index-group=Unrevised
type=page
status=published
title=Details on openejb-jar
~~~~~~

<a name="Detailsonopenejb-jar-Whatisanopenejb-jar.xml?"></a>
# What is an openejb-jar.xml?

This is the file created by the Deploy Tool that maps your bean's
deployment descriptor (ejb-jar.xml) to actual containers and resources
declared in your OpenEJB configuration (openejb.conf). In fact, the Deploy
tool really does nothing more than create this file and put it in your jar,
that's it.

<a name="Detailsonopenejb-jar-Whenistheopenejb-jar.xmlused?"></a>
# When is the openejb-jar.xml used?

At startup, any jar containing a openejb-jar.xml is loaded by the container
system. The configuration tools will go looking in all the directories and
jars you have declared in your openejb.conf with the <Deployment> element.
For every jar file it finds, it will look inside for an openejb-jar.xml. If
it finds one, it will attempt to load and deploy it into the container
system.

<a name="Detailsonopenejb-jar-DoIevenneedthedeploytoolthen?"></a>
# Do I even need the deploy tool then?

Nope. Typically you would only use the deploy tool to create your
openejb-jar.xml, then just keep your openejb-jar.xml in your CVS (or other
repository). If you learn how to maintain this openejb-jar.xml file, you'll
never need the deploy tool again! You can do all your builds and deploys
automatically.

<a name="Detailsonopenejb-jar-WheredoIputtheopenejb-jar.xmlinmyjar?"></a>
# Where do I put the openejb-jar.xml in my jar?

The openejb-jar.xml file just goes in the META-INF directory of your jar
next to the ejb-jar.xml file.

<a name="Detailsonopenejb-jar-Isthefileformateasy?"></a>
# Is the file format easy?

If you can understand the ejb-jar.xml, the openejb-jar.xml should be a
breeze.

This is the openejb-jar.xml that is created by the Deploy tool in the Hello
World example. As you can see, the file format is extremely simple.

    <?xml version="1.0"?>
    <openejb-jar xmlns="http://www.openejb.org/openejb-jar/1.1">
        <ejb-deployment  ejb-name="Hello"
             deployment-id="Hello"
             container-id="Default Stateless Container"/>
    </openejb-jar>

    
    
The *ejb-name* attribute is the name you gave the bean in your ejb-jar.xml.
The *deployment-id* is the name you want to use to lookup the bean in your
client's JNDI namespace. The *container-id* is the name of the container in
your openejb.conf file that you would like the bean to run in. There MUST
be one *ejb-deployment* element for each EJB in your jar.
    
# What if my bean uses a JDBC datasource?
    
Then you simply add a <resource-link> element to your <ejb-deployment>
element like this
    
    <?xml version="1.0"?>
    <openejb-jar xmlns="http://www.openejb.org/openejb-jar/1.1">
        
        <ejb-deployment  ejb-name="Hello" 
    		     deployment-id="Hello" 
    		     container-id="Default Stateless Container" >
             
    	<resource-link res-ref-name="jdbc/basic/entityDatabase" 
    		     res-id="Default JDBC Database"/>
        
        </ejb-deployment>
    
    </openejb-jar>



The *res-ref-name* attribute refers to the <res-ref-name> element of the
bean's <resource-ref> declaration in the ejb-jar.xml. The *res-id*
attribute refers to the id of the <Connector> declared in your openejb.conf
that will handle the connections and provide access to the desired
resource.

<a name="Detailsonopenejb-jar-Howmanyresource-linkelementswillIneed?"></a>
# How many resource-link elements will I need?

You will need one <resource-link> element for every <resource-ref> element
in your ejb-jar.xml. So if you had an ejb-jar.xml like the following

    <?xml version="1.0"?>
    <ejb-jar>
      <enterprise-beans>
        <session>
          <ejb-name>MyExampleBean</ejb-name>
          <home>com.widget.ExampleHome</home>
          <remote>com.widget.ExampleObject</remote>
          <ejb-class>com.widget.ExampleBean</ejb-class>
          <session-type>Stateless</session-type>
          <transaction-type>Container</transaction-type>

          <resource-ref>
            <description>
              This is a reference to a JDBC database.
            </description>
            <res-ref-name>jdbc/myFirstDatabase</res-ref-name>
            <res-type>javax.sql.DataSource</res-type>
            <res-auth>Container</res-auth>
          </resource-ref>

          <resource-ref>
            <description>
              This is another reference to a JDBC database.
            </description>
            <res-ref-name>jdbc/anotherDatabase</res-ref-name>
            <res-type>javax.sql.DataSource</res-type>
            <res-auth>Container</res-auth>
          </resource-ref>

        </session>
      </enterprise-beans>
    </ejb-jar>

    
Then you would need two <resource-link> elements for that bean in your
openejb-jar.xml file as such.
    
    <?xml version="1.0"?>
    <openejb-jar xmlns="http://www.openejb.org/openejb-jar/1.1">
        
        <ejb-deployment  ejb-name="MyExampleBean" 
    		     deployment-id="MyExampleBean" 
    		     container-id="Default Stateless Container" >
             
    	<resource-link res-ref-name="jdbc/myFirstDatabase" 
    		     res-id="My Oracle JDBC Database"/>
    
    	<resource-link res-ref-name="jdbc/anotherDatabase" 
    		     res-id="My PostgreSQL JDBC Database"/>
        
        </ejb-deployment>
    
    </openejb-jar>



This would require two <Connector> declarations in your openejb.conf, one
with the *id* attribute set to _"My Oracle JDBC Database"_ , and another
with it's *id* attribute set to _"My PostgreSQL JDBC Database"_ 
