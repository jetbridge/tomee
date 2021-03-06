index-group=ActiveMQ
type=page
status=published
title=ActiveMQResourceAdapter Configuration
~~~~~~


A ActiveMQResourceAdapter can be declared via xml in the `<tomee-home>/conf/tomee.xml` file or in a `WEB-INF/resources.xml` file using a declaration like the following.  All properties in the element body are optional.

    <Resource id="myActiveMQResourceAdapter" type="ActiveMQResourceAdapter">
        brokerXmlConfig = broker:(tcp://localhost:61616)?useJmx=false
        dataSource = Default Unmanaged JDBC Database
        serverUrl = vm://localhost?waitForStart=20000&async=true
        startupTimeout = 10 seconds
    </Resource>

Alternatively, a ActiveMQResourceAdapter can be declared via properties in the `<tomee-home>/conf/system.properties` file or via Java VirtualMachine `-D` properties.  The properties can also be used when embedding TomEE via the `javax.ejb.embeddable.EJBContainer` API or `InitialContext`

    myActiveMQResourceAdapter = new://Resource?type=ActiveMQResourceAdapter
    myActiveMQResourceAdapter.brokerXmlConfig = broker:(tcp://localhost:61616)?useJmx=false
    myActiveMQResourceAdapter.dataSource = Default Unmanaged JDBC Database
    myActiveMQResourceAdapter.serverUrl = vm://localhost?waitForStart=20000&async=true
    myActiveMQResourceAdapter.startupTimeout = 10 seconds

Properties and xml can be mixed.  Properties will override the xml allowing for easy configuration change without the need for ${} style variable substitution.  Properties are not case sensitive.  If a property is specified that is not supported by the declared ActiveMQResourceAdapter a warning will be logged.  If a ActiveMQResourceAdapter is needed by the application and one is not declared, TomEE will create one dynamically using default settings.  Multiple ActiveMQResourceAdapter declarations are allowed.

## Supported Properties
<table class="mdtable">
<tr>
<th>Property</th>
<th>Type</th>
<th>Default</th>
<th>Description</th>
</tr>
<tr>
  <td>brokerXmlConfig</td>
  <td>String</td>
  <td>broker:(tcp://localhost:61616)?useJmx=false</td>
  <td>
Broker configuration URI as defined by ActiveMQ
see http://activemq.apache.org/broker-configuration-uri.html
BrokerXmlConfig xbean:file:conf/activemq.xml - Requires xbean-spring.jar and dependencies
</td>
</tr>
<tr>
  <td>dataSource</td>
  <td>String</td>
  <td>Default&nbsp;Unmanaged&nbsp;JDBC&nbsp;Database</td>
  <td>
DataSource for persistence messages
</td>
</tr>
<tr>
  <td>serverUrl</td>
  <td>java.net.URI</td>
  <td>vm://localhost?waitForStart=20000&async=true</td>
  <td>
Broker address
</td>
</tr>
<tr>
  <td>startupTimeout</td>
  <td><a href="configuring-durations.html">time</a></td>
  <td>10&nbsp;seconds</td>
  <td>
How long to wait for broker startup
</td>
</tr>
</table>
