index-group=Unrevised
type=page
status=published
title=Getting Started
~~~~~~
##The following instructions are written using Eclipse 3.2. We will
refer to the install location of OpenEJB as OPENEJB_HOME

Here are some basic steps you need to perform to get started with OpenEJB
1. Download and install OpenEJB
1. Setup your development environment
1. Write an EJB
1. Write an EJB client
1. Start the server
1. Deploy the EJB
1. Run the client
1. Stop the server

> 

<a name="GettingStarted-&nbsp;1.DownloadandInstallOpenEJB"></a>
##1. Download and Install OpenEJB

Follow these&nbsp;[instructions](http://cwiki.apache.org/confluence/display/OPENEJB/Quickstart)

<a name="GettingStarted-&nbsp;2.Setupyourdevelopmentenvironment"></a>
##2. Setup your development environment


<a name="GettingStarted-&nbsp;Eclipse"></a>
###Eclipse

- Open eclipse and create a new java project. Name it EJBProject
- Add the following jars to the build path of your project
-- OPENEJB_HOME/lib/geronimo-ejb_3.0_spec-1.0.jar
- Now create another project named EJBClient. This is where we will write a test client
- Add the following jars to the build path of this project
-- OPENEJB_HOME/lib/openejb-client-3.0.0-SNAPSHOT.jar
- Add the EJBProject to the classpath of the EJBClient project

<a name="GettingStarted-&nbsp;3.StarttheServer"></a>
##3. Start the Server

Open the command prompt and run the following command:

    d:\openejb-3.0.0-SNAPSHOT\bin\openejb start

You will get the following message on the console:

    D:\openejb-3.0.0-SNAPSHOT>bin\openejb start
    Apache OpenEJB 3.0.0-SNAPSHOT	 build: 20070830-07:53
    http://tomee.apache.org/
    OpenEJB ready.
    [OPENEJB:init]
     OpenEJB Remote Server
          ** Starting Services **
          NAME		       IP	       PORT
          httpejbd	       0.0.0.0	       4204
          admin thread	       0.0.0.0	       4200
          ejbd		       0.0.0.0	       4201
          hsql		       0.0.0.0	       9001
          telnet	       0.0.0.0	       4202
        -------
        Ready!


<a name="GettingStarted-&nbsp;4.WriteanEJB"></a>
##4. Write an EJB

In the EJB project create a new interface named Greeting

    package com.myejbs;
    
    import javax.ejb.Remote;
    
    @Remote
    public interface Greeting {
      public String greet();
    }

Now create a new class named GreetingBean which implements the above
interface (shown below)

    package com.myejbs;
    
    import javax.ejb.Stateless;
    
    @Stateless
    public class GreetingBean implements Greeting {
    
    	public String greet() {
    		return "My First Remote Stateless Session Bean";
    	}
    
    }


<a name="GettingStarted-&nbsp;5.DeploytheEJB"></a>
## 5. Deploy the EJB

1. Export the EJBProject as a jar file. Name it greeting.jar and put it in
the OPENEJB_HOME/apps directory.
1. Open the command prompt and type in the following command:


    d:\openejb-3.0.0-SNAPSHOT > bin\openejb deploy apps\greeting.jar

This should give you the following output:

    D:\openejb-3.0.0-SNAPSHOT>bin\openejb deploy apps\greeting.jar
    
    Application deployed successfully at \{0\}
    
    App(id=D:\openejb-3.0.0-SNAPSHOT\apps\greeting.jar)
    
        EjbJar(id=greeting.jar, path=D:\openejb-3.0.0-SNAPSHOT\apps\greeting.jar)
    
    	Ejb(ejb-name=GreetingBean, id=GreetingBean)
    
    	    Jndi(name=GreetingBeanRemote)

{color:#330000}{*}Notice the Jndi(name=GreetingBeanRemote) information.
Keep this handy as this is the JNDI name of the bean which the client will
use for lookup{*}{color}

<a name="GettingStarted-&nbsp;6.WritetheClient"></a>
##6. Write the Client

In the EJBClient project, create a class named Client (shown below)

    package com.myclient;
    
    import com.myejbs.Greeting;
    
    import javax.naming.InitialContext;
    import java.util.Properties;
    
    public class Client {
        public static void main(String[] args) {
    
            try {
                Properties p = new Properties();
                p.put("java.naming.factory.initial", "org.openejb.client.RemoteInitialContextFactory");
                p.put("java.naming.provider.url", "ejbd://127.0.0.1:4201");
                InitialContext ctx = new InitialContext(p);
                Greeting greeter = (Greeting) ctx.lookup("GreetingBeanRemote");
                String message = greeter.greet();
                System.out.println(message);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }


<a name="GettingStarted-&nbsp;7.RuntheClient"></a>
##7. Run the Client

Open Client.java in eclipse and run it as a java application. You should
see the following message in the console view:

    My First Remote Stateless Session Bean


<a name="GettingStarted-&nbsp;8.Stoptheserver"></a>
##8. Stop the server

There are two ways to stop the server:
1. You can press Ctrl+c on the command prompt to stop the server
1. On the command prompt type in the following command:

    D:\openejb-3.0.0-SNAPSHOT>bin\openejb stop
