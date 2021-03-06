index-group=Unrevised
type=page
status=published
title=TomEE and WebSphere MQ
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

**Steps to integrate TomEE with Websphere MQ** 

1. Unzip rar file place jars under tomee/lib

2. Added the below to conf/tomee.xml
    
<pre>
    &lt;tomee&gt;     
    &lt;Container id=&quot;wmq&quot; type=&quot;MESSAGE&quot;&gt;
    ResourceAdapter=wmqRA
    MessageListenerInterface=javax.jms.MessageListener
    ActivationSpecClass=com.ibm.mq.connector.inbound.ActivationSpecImpl
    &lt;/Container&gt;


   &lt;Resource id=&quot;wmqRA&quot; type=&quot;com.ibm.mq.connector.ResourceAdapterImpl&quot; class-name=&quot;com.ibm.mq.connector.ResourceAdapterImpl&quot;&gt;
    connectionConcurrency=5  
    maxConnections=10 
    logWriterEnabled=true 
    reconnectionRetryCount=5 
    reconnectionRetryInterval=300000 
    traceEnabled=false 
    traceLevel=3 
   &lt;/Resource&gt;

   &lt;Resource **id=&quot;qcf&quot;**  type=&quot;javax.jms.ConnectionFactory&quot; class-name=&quot;com.ibm.mq.connector.outbound.ManagedConnectionFactoryImpl&quot;&gt;
    TransactionSupport=none 
    ResourceAdapter=wmqRA 
    HostName=10.a.b.c   
    Port=1414 
    QueueManager=QM_TIERL
   Channel=SYSTEM.ADMIN.SVRCONN
   TransportType=Client
   UserName=xyz
   Password=*****
  &lt;/Resource&gt;

  &lt;Resource id=&quot;wmq-javax.jms.QueueConnectionFactory&quot;  type=&quot;javax.jms.QueueConnectionFactory&quot; class-name=&quot;com.ibm.mq.connector.outbound.ManagedQueueConnectionFactoryImpl&quot;&gt;
    TransactionSupport=xa 
    ResourceAdapter=wmqRA 
  &lt;/Resource&gt;

  &lt;Resource id=&quot;wmq-javax.jms.TopicConnectionFactory&quot;  type=&quot;javax.jms.TopicConnectionFactory&quot; class-name=&quot;com.ibm.mq.connector.outbound.ManagedTopicConnectionFactoryImpl&quot;&gt;
    TransactionSupport=xa 
    ResourceAdapter=wmqRA 
  &lt;/Resource&gt;

  &lt;Resource **id=&quot;queue&quot;** type=&quot;javax.jms.Queue&quot;  
class-name=&quot;com.ibm.mq.connector.outbound.MQQueueProxy&quot;&gt; 
    arbitraryProperties 
    baseQueueManagerName 
    baseQueueName 
    CCSID=1208 
    encoding=NATIVE 
    expiry=APP 
    failIfQuiesce=true 
    persistence=APP 
    priority=APP 
    readAheadClosePolicy=ALL 
    targetClient=JMS 
  &lt;/Resource&gt;

  &lt;Resource id=&quot;wmq-javax.jms.Topic&quot; type=&quot;javax.jms.Topic&quot; class-name=&quot;com.ibm.mq.connector.outbound.MQTopicProxy&quot;&gt;
    arbitraryProperties 
    baseTopicName 
    brokerCCDurSubQueue=SYSTEM.JMS.D.CC.SUBSCRIBER.QUEUE 
    brokerDurSubQueue=SYSTEM.JMS.D.SUBSCRIBER.QUEUE 
    brokerPubQueue 
    brokerPubQueueManager 
    brokerVersion=1 
    CCSID=1208 
    encoding=NATIVE 
    expiry=APP 
    failIfQuiesce=true 
    persistence=APP 
    priority=APP 
    readAheadClosePolicy=ALL 
    targetClient=JMS 
  &lt;/Resource&gt; 

 &lt;/tomee&gt;	 

3. In web.xml add the below to access resources
 &lt;resource-ref&gt; 
     &lt;res-ref-name&gt;myqcf&lt; /res-ref-name&gt; 
    &lt;res-type&gt;javax.jms.ConnectionFactory &lt; /res-type&gt;
    &lt;res-auth&gt;Container&lt;/res-auth&gt;&lt; /br&gt;
    &lt;res-sharing-scope&gt;Shareable&lt; /res-sharing-scope&gt;
    &lt;mapped-name&gt;qcf&lt; /mapped-name&gt;
  &lt;/resource-ref&gt;
  
 &lt;resource-env-ref&gt;
   &lt;resource-env-ref-name&gt;myqueue&lt; /resource-env-ref-name&gt;
   &lt;resource-env-ref-type&gt;javax.jms.Queue&lt; /resource-env-ref-type&gt;
   &lt;mapped-name&gt;queue&lt; /mapped-name&gt;
  &lt;/resource-env-ref&gt;
</pre>

**Code:**
<pre>    
    @Resource(name = &quot;qcf&quot;) 
    private ConnectionFactory connectionFactory; 
    @Resource(name = &quot;queue&quot;) 
    private Queue queue;
    Connection connection = connectionFactory.createConnection();
    Session session = connection.createSession(false, QueueSession.AUTO_ACKNOWLEDGE);
    MessageProducer producer = session.createProducer(queue);
    TextMessage message = session.createTextMessage();
    message.setText(&quot;Test Message&quot;);
    connection.start();
    producer.send(message);
    session.close();
    connection.close();
</pre>
