index-group=Unrevised
type=page
status=published
title=EJB Request Logging
~~~~~~
Notice:    Licensed to the Apache Software Foundation (ASF) under one
           or more contributor license agreements.  See the NOTICE file
           distributed with this work for additional information
           regarding copyright ownership.  The ASF licenses this file
           to you under the Apache License, Version 2.0 (the
           "License"); you may not use this file except in compliance
           with the License.  You may obtain a copy of the License at
           .
             http://www.apache.org/licenses/LICENSE-2.0
           .
           Unless required by applicable law or agreed to in writing,
           software distributed under the License is distributed on an
           "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
           KIND, either express or implied.  See the License for the
           specific language governing permissions and limitations
           under the License.

Both client-side and server-side request logging is supported and includes various invocation times aimed at helping to identify where time
is going in a request.

## Client-side

On the client requests/responses are logged on java.util.logging `FINEST` level in category `OpenEJB.client`.  The code is similar to the following:

    final long time = System.nanoTime() - start;
    final String message = String.format("Invocation %sns - %s - Request(%s) - Response(%s)", time, conn.getURI(), req, res);
    logger.log(Level.FINEST, message);

Note that the check to see if FINEST is enabled is cached for performance reasons, so it must be set at VM startup.

## Server-side

On the server requests/responses are logged in java.util.logging `FINE` in category`"OpenEJB.server.remote.ejb`.  The code for that
is similar to this:

    logger.fine("EJB REQUEST: " + req + " -- RESPONSE: " + res);

## Request times

Three times are tracked per request and logged in the above statements as part of the formatting of the EJB Response.  They're best understood in reverse:

 - **Container time** -- this is the raw time of invoking the bean including its 
interceptors and any transaction begin/commit time.  This time is effectively "business logic".
 - **Server time** -- this timer starts the (nano)second the request is seen by the server and
 the (nano)second the response is actually written.  The serverTime minus the containerTime will 
effectively show you how long serialization and deserialization is taking pre request.
 - **Client time** -- entire begin to end including attempting to contact the server and fully reading the response.  The clientTime minus the serverTime will show network latency for the most part.

The container time and server time are written in the EJB response and visible to the client.  The client will log all three times, the server will log the first two.  All log statements are on a single line.

## Bean-time and JMX Statistics

The above information applies purely to remote EJB calls made over a network.  Calls on `@Remote` or `@Local` interfaces between two components in the same server are not logged.

However, **all** EJB invocations to business methods *or* callbacks like `@PostConstruct` are tracked for statistical analysis.
By default a floating window of 2000 samples are kept.  The time tracked is purely **bean time** which includes 
interceptors, decorators and the bean itself, but does not include other container services like transactions or security.  

This information is available in JMX.  A sample JMX ObjectName for a `CounterBean` will look like this:

    openejb.management:J2EEServer=openejb,J2EEApplication=null,EJBModule=StatsModule,StatelessSessionBean=CounterBean,j2eeType=Invocations,name=CounterBean

All beans have the following MBean attributes, listed here in shorthand:

 * javax.management.MBeanAttributeInfo[description=, name=InvocationCount, type=long, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=InvocationTime, type=long, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=MonitoredMethods, type=long, read-only, descriptor={}]
 * javax.management.MBeanOperationInfo[description=, name=FilterAttributes, returnType=void, signature=[javax.management.MBeanParameterInfo[description="", name=excludeRegex, type=java.lang.String, descriptor={}], javax.management.MBeanParameterInfo[description="", name=includeRegex, type=java.lang.String, descriptor={}]], impact=unknown, descriptor={}]

Then for every method there will be these attributes and operations:

 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Count, type=long, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().GeometricMean, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Kurtosis, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Max, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Mean, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Min, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile01, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile10, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile25, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile50, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile75, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile90, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Percentile99, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().SampleSize, type=int, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Skewness, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().StandardDeviation, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Sum, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Sumsq, type=double, read-only, descriptor={}]
 * javax.management.MBeanAttributeInfo[description=, name=someMethod().Variance, type=double, read-only, descriptor={}]
 * javax.management.MBeanOperationInfo[description=, name=someMethod().setSampleSize, returnType=void, signature=[javax.management.MBeanParameterInfo[description=, name=p1, type=int, descriptor={}]], impact=unknown, descriptor={}]
 * javax.management.MBeanOperationInfo[description=, name=someMethod().sortedValues, returnType=[D, signature=[], impact=unknown, descriptor={}]
 * javax.management.MBeanOperationInfo[description=, name=someMethod().values, returnType=[D, signature=[], impact=unknown, descriptor={}]
