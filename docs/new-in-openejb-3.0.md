title= New in OpenEJB 3.0
type=page
status=published
~~~~~~

<a name="NewinOpenEJB3.0-EJB3.0"></a>
# EJB 3.0

OpenEJB 3.0 supports the EJB 3.0 specification as well as the prior EJB
2.1, EJB 2.0, and EJB 1.1.  New features in EJB 3.0 include:

 - Annotations instead of xml
 - No home interfaces
 - Business Interfaces
 - Dependency Injection
 - Intercpetors
 - Java Persistence API
 - Service Locator (ala SessionContext.lookup)
 - POJO-style beans

EJB 2.x features since OpenEJB 1.0 also include:
 - MessageDriven Beans
 - Container-Managed Persistence (CMP) 2.0
 - Timers

The two aspects of EJB that OpenEJB does not yet support are:
  - Web Services (JAX-WS, JAX-RPC)
  - CORBA

JAX-WS and CORBA support will be added in future releases.  Support for the
JAX-RPC API is not a planned feature.

<a name="NewinOpenEJB3.0-ExtensionstoEJB3.0"></a>
# Extensions to EJB 3.0

<a name="NewinOpenEJB3.0-CMPviaJPA"></a>
## CMP via JPA

Our CMP implementation is a thin layer over the new Java Persistence API
(JPA).	This means when you deploy an old style CMP 1.1 or CMP 2.1 bean it
is internally converted and ran as a JPA bean.	This makes it possible to
use both CMP and JPA in the same application without any coherence issues
that can come from using two competing persistence technologies against the
same data.  Everything is ultimately JPA in the end.

<a name="NewinOpenEJB3.0-ExtendedDependencyInjection"></a>
## Extended Dependency Injection

Dependency Injection in EJB 3.0 via @Resource is largely limited to objects
provided by the container, such as DataSources, JMS Topics and Queues.	It
is possible for you to supply your own configuration information for
injection, but standard rules allow for only data of type String,
Character, Boolean, Integer, Short, Long, Double, Float and Byte.  If you
needed a URL, for example, you'd have to have it injected as a String then
convert it yourself to a URL.  This is just plain silly as the conversion
of Strings to other basic data types has existed in JavaBeans long before
Enterprise JavaBeans existed.  

OpenEJB 3.0 supports injection of any data type for which you can supply a
JavaBeans PropertyEditor.  We include several built-in PropertyEditors
already such as Date, InetAddress, Class, File, URL, URI, Map, List and
more.


    import java.net.URI;
    import java.io.File;
    import java.util.Date;
    
    @Stateful 
    public class MyBean {
        @Resource URI blog;
        @Resource Date birthday;
        @Resource File homeDirectory;
    }


<a name="NewinOpenEJB3.0-TheMETA-INF/env-entries.properties"></a>
## The META-INF/env-entries.properties

Along the lines of injection, one of the last remaining things in EJB 3
that people need an ejb-jar.xml file for is to supply the value of
env-entries.  Env Entries are the source of data for all user supplied data
injected into your bean; the afore mentioned String, Boolean, Integer, etc.
 This is a very big burden as each env-entry is going to cost you 5 lines
of xml and the complication of having to figure out how to add you bean
declaration in xml as an override of an existing bean and not accidentally
as a new bean.	All this can be very painful when all you want is to supply
the value of a few @Resource String fields in you bean class.  

To fix this, OpenEJB supports the idea of a META-INF/env-entries.properties
file where we will look for the value of things that need injection that
are not container controlled resources (i.e. datasources and things of that
nature).  You can configure you ejbs via a properties file and skip the
need for an ejb-jar.xml and it's 5 lines per property madness.


    blog = http://acme.org/myblog
    birthday = locale=en_US style=MEDIUM Mar 1, 1954
    homeDirectory = /home/esmith/


<a name="NewinOpenEJB3.0-SupportforGlassFishdescriptors"></a>
## Support for GlassFish descriptors

Unit testing EJBs with OpenEJB is a major feature and draw for people, even
for people who may still use other app servers for final deployment such as
Geronimo or GlassFish.	The descriptor format for Geronimo is natively
understood by OpenEJB as OpenEJB is the EJB Container provider for
Geronimo.  However, OpenEJB also supports the GlassFish descriptors so
people using GlassFish as their final server can still use OpenEJB for
testing EJBs via plain JUnit tests in their build and only have one set of
vendor descriptors to maintain.

<a name="NewinOpenEJB3.0-JavaEE5EARandApplicationClientsupport"></a>
## JavaEE 5 EAR and Application Client support

JavaEE 5 EARs and Application Clients can be deployed in addition to ejb
jars.  EAR support is limited to ejbs, application clients, and libraries;
WAR files and RAR files will be ignored.   Per the JavaEE 5 spec, the
META-INF/application.xml and META-INF/application-client.xml files are
optional.

<a name="NewinOpenEJB3.0-ApplicationValidationforEJB3.0"></a>
##  Application Validation for EJB 3.0

Incorrect usage of various new aspects of EJB 3.0 are checked for and
reported during the deployment process preventing strange errors and
failures.  

As usual validation failures (non-compliant issues with your application)
are printed out in complier-style "all-at-once" output allowing you to see
and fix all your issues in one go.  For example, if you have 10
@PersistenceContext annotations that reference an invalid persistence unit,
you get all 10 errors on the *first* deploy rather than one failure on the
first deploy with 9 more failed deployments to go.

Validation output comes in three levels.  The most verbose level will tell
you in detail what you did wrong, what the options are, and what to do
next... even including valid code and annotation usage tailored to your app
that you can copy and paste into your application.  Very ideal for
beginners and people using OpenEJB in a classroom setting.

<a name="NewinOpenEJB3.0-MostconfigurableJNDInamesever"></a>
##  Most configurable JNDI names ever

<a name="NewinOpenEJB3.0-GeneralImprovements"></a>
# General Improvements

<a name="NewinOpenEJB3.0-OnlineDeployment"></a>
##  Online Deployment
<a name="NewinOpenEJB3.0-SecurityService"></a>
##  Security Service
<a name="NewinOpenEJB3.0-ConnectionPooling"></a>
##  Connection Pooling
<a name="NewinOpenEJB3.0-ConfigurationOverriding"></a>
##  Configuration Overriding
<a name="NewinOpenEJB3.0-FlexibleJNDINameFormatting"></a>
##  Flexible JNDI Name Formatting
<a name="NewinOpenEJB3.0-CleanerEmbedding"></a>
##  Cleaner Embedding
<a name="NewinOpenEJB3.0-Tomcat6Support"></a>
##  Tomcat 6 Support
<a name="NewinOpenEJB3.0-Businesslocalsremotable"></a>
##  Business locals remotable

If you want to make business local interfaces remotable, you can set this
property:

      openejb.remotable.businessLocals=true

Then you can lookup your business local interfaces from remote clients.

You'd still have to ensure that the you pass back and forth is
serializable.





