index-group=Unrevised
type=page
status=published
title=Installing Bouncy Castle
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

Installation of Bouncy Castle for use in TomEE itself is done in two steps:

 1. Add the Bouncy Castle provider jar to the `$JAVA_HOME/jre/lib/ext` directory
 1. Create a Bouncy Castle provider entry in the  `$JAVA_HOME/jre/lib/security/java.security` file

The entry to `java.security` will look something like the following:

    security.provider.N=org.bouncycastle.jce.provider.BouncyCastleProvider

Replace `N` with the order of precedence you would like to give Bouncy Castle in comparison to the
other providers in the file.  **Recommended** would be the last entry in the list -- `N` being the higest number in the list.
**Warning** that configuring Bouncy Castle as the first provider, `security.provider.1`, may cause JVM errors.
