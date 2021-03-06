index-group=Unrevised
type=page
status=published
title=Remote Server
~~~~~~

!http://www.openejb.org/images/diagram-remote-server.gif|valign=top,
align=right, hspace=15!
<a name="RemoteServer-AccessingEJBsRemotely"></a>
# Accessing EJBs Remotely

When using OpenEJB as a stand-alone server you can connect across a network
and access EJBs from a remote client.  The client code for accessing an
EJB's Remote Interface is the same, however to actually connect across a
network to the server, you need to specify different JNDI parameters.

<a name="RemoteServer-Shortversion"></a>
# Short version

Using OpenEJB's default remote server implementation is pretty straight
forward. You simply need to:

1. Deploy your bean.
1. Start the server on the IP and Port you want, 25.14.3.92 and 4201 for
example.
1. Use that information in your client to create an initial context
1. Add the right jars to your client's classpath

So, here it is in short.

Deploy your bean with the Deploy Tool:

    c:\openejb> openejb.bat deploy beans\myBean.jar

See the [OPENEJBx30:Deploy Tool](openejbx30:deploy-tool.html)
 documentation for more details on deploying beans.

Start the server:

    c:\openejb> openejb.bat start -h 25.14.3.92 -p 4201

See the Remote Server command-line guide for more details on starting the
Remote Server.

Create an initial context in your client as such:


    Properties p = new Properties();
    p.put("java.naming.factory.initial", "org.apache.openejb.client.RemoteInitialContextFactory");
    p.put("java.naming.provider.url", "ejbd://25.14.3.92:4201");
    p.put("java.naming.security.principal", "myuser");
    p.put("java.naming.security.credentials", "mypass");
        
    InitialContext ctx = new InitialContext(p);


If you don't have any EJBs or clients to run, try the ubiquitous [Hello World](openejbx30:hello-world.html)
 example.
Add the following library to your clients classpath:

* openejb-client-x.x.x.jar
* javaee-api-x.x.jar

Both can be found in the lib directory where you installed OpenEJB or in Maven repositories.
