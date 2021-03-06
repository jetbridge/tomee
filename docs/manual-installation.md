index-group=Unrevised
type=page
status=published
title=Manual Installation
~~~~~~

# Overview

The manual installation process is significantly harder then the [automatic installation](tomcat.html)
 which we normally recommend.  In this installation process you will do the
following:

1. Install openejb.war
1.  Download openejb.war from the [download page](http://tomee.apache.org/downloads.html)
1.  Make webapps/openejb directory
1.  Change to new webapps/openejb directory
1.  Unpack the openejb.war file in the new directory
1. Add the OpenEJB listener the conf/server.xml file
1. Update the non-compliant Tomcat annotations-api.jar
1. Add the OpenEJB JavaAgent to the bin/catalina.bat or bin/catalina.bat
script

##Install openejb.war

Once Tomcat has been [installed](tomcat-installation.html)
, the OpenEJB plugin for Tomcat can be installed.  The war can be obtained
from the [download page](http://tomee.apache.org/downloads.html)

The commands in this example are executed from within the Tomcat
installation directory.

<a name="ManualInstallation-UnpackOpenEJBTomcatplugininTomcatwebappsdirectory"></a>
## Unpack OpenEJB Tomcat plugin in Tomcat webapps directory

Be careful, this is the most error prone step.  A web
application does not contain a root directory, so if you unpack it in the
wrong directory, it is difficult to undo.  Please, follow this step
closely, and most importantly make sure you execute the unpack command
from within the new webapps/openejb directory

Due to the structure of war files, you must create a new directory for
OpenEJB, change to the new directory and execute the unpack command from
within the new directory.  If you get this wrong, it is difficult to undo,
so follow the steps closely.

<pre><code>
    C:\apache-tomcat-6.0.14>mkdir webapps\openejb
    
    C:\apache-tomcat-6.0.14>cd webapps\openejb
    
    C:\apache-tomcat-6.0.14\webapps\openejb>jar -xvf \openejb.war
      created: WEB-INF/
      created: WEB-INF/classes/
      created: WEB-INF/classes/org/
      created: WEB-INF/classes/org/apache/
      created: WEB-INF/classes/org/apache/openejb/
    ...snip...
    
    C:\apache-tomcat-6.0.14\webapps\openejb>dir
     Volume in drive C has no label.
     Volume Serial Number is 0000-0000
    
     Directory of C:\apache-tomcat-6.0.14\webapps\openejb
    
    09/21/2007  10:19 AM	<DIR>	       .
    09/21/2007  10:19 AM	<DIR>	       ..
    09/21/2007  10:19 AM		 1,000 index.html
    09/21/2007  10:19 AM	<DIR>	       lib
    09/21/2007  10:19 AM		11,358 LICENSE
    09/21/2007  10:19 AM	<DIR>	       META-INF
    09/21/2007  10:19 AM		11,649 NOTICE
    09/21/2007  10:19 AM		 1,018 openejb.xml
    09/21/2007  10:19 AM		 1,886 README.txt
    09/21/2007  10:19 AM	<DIR>	       tomcat
    09/21/2007  10:19 AM	<DIR>	       WEB-INF
    	       5 File(s)	 26,911 bytes
    	       6 Dir(s)   4,633,796,608 bytes free
    
    C:\apache-tomcat-6.0.14\webapps\openejb>cd ..\..
    
    C:\apache-tomcat-6.0.14>
    
        {card:label=Unix}{noformat:nopanel=true}
        apache-tomcat-6.0.14$ mkdir webapps/openejb
        
        apache-tomcat-6.0.14$ cd webapps/openejb/
        
        apache-tomcat-6.0.14/webapps/openejb$ jar -xvf path/to/openejb.war 
          created: WEB-INF/
          created: WEB-INF/classes/
          created: WEB-INF/classes/org/
          created: WEB-INF/classes/org/apache/
          created: WEB-INF/classes/org/apache/openejb/
        ...snip...
        
        apache-tomcat-6.0.14/webapps/openejb$ ls
        LICENSE      META-INF/	  NOTICE       README.txt   WEB-INF/	 index.html
      lib/	       openejb.xml  tomcat/
        
        apache-tomcat-6.0.14/webapps/openejb$ cd ../..
        
        apache-tomcat-6.0.14$

</code></pre>

<a name="ManualInstallation-AddtheOpenEJBlistenertoTomcat"></a>
## Add the OpenEJB listener to Tomcat 

All Tomcat listener classes must be available in the Tomcat common class
loader, so the openejb-loader jar must be copied into the Tomcat lib
directory.


        C:\apache-tomcat-6.0.14>copy webapps\openejb\lib\openejb-loader-3.0.0-SNAPSHOT.jar lib\openejb-loader.jar
    	1 file(s) copied.
    
        apache-tomcat-6.0.14$ cp webapps/openejb/lib/openejb-loader-*.jar lib/openejb-loader.jar


Add the following `<Listener
className="org.apache.openejb.loader.OpenEJBListener" />` to your conf/server.xml file to load the OpenEJB listener:

The snippet is shown below
 
    <!-- Note:  A "Server" is not itself a "Container", so you may not
    define subcomponents such as "Valves" at this
    level.
    Documentation at /docs/config/server.html
     -->

    <Server port="8005" shutdown="SHUTDOWN">
    <!-- OpenEJB plugin for tomcat -->
    <Listener
    className="org.apache.openejb.loader.OpenEJBListener" />

    <!--APR library loader. Documentation at /docs/apr.html -->    
    <Listener
    className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />

<a name="ManualInstallation-UpdatetheTomcatannotations-api.jarfile"></a>
## Update the Tomcat annotations-api.jar file

Tomcat contains an old non-compliant version of the javax.annotation
classes and these invalid classes must be updated so OpenEJB can process
annotations.  Simply, replace the annotations-api.jar in the Tomcat lib
directory with the updated annotations-api.jar in the OpenEJB war.

<pre><code>

C:\apache-tomcat-6.0.14>copy webapps\openejb\tomcat\annotations-api.jar
lib\annotations-api.jar
Overwrite lib\annotations-api.jar? (Yes/No/All): y
	1 file(s) copied.

apache-tomcat-6.0.14$ cp webapps/openejb/tomcat/annotations-api.jar
lib/annotations-api.jar 

</code></pre>

<a name="ManualInstallation-{anchor:javaagent}AddOpenEJBjavaagenttoTomcatstartup"></a>
## Add OpenEJB javaagent to Tomcat startup

OpenJPA, the Java Persistence implementation used by OpenEJB, currently
must enhanced persistence classes to function properly, and this requires
the installation of a javaagent into the Tomcat startup process.

First, copy the OpenEJB JavaAgent jar into the lib directory.

<pre><code>

    C:\apache-tomcat-6.0.14>copy webapps\openejb\lib\openejb-javaagent-3.0.0-SNAPSHOT.jar lib\openejb-javaagent.jar
    	1 file(s) copied.
    
    apache-tomcat-6.0.14$ cp webapps/openejb/lib/openejb-javaagent-*.jar lib/openejb-javaagent.jar

</code></pre>

Simply, add the snippet marked below in
bin/catalina.bat (Windows) or bin/catalina.sh (Unix) file to enable the
OpenEJB javaagent:

    if not exist "%CATALINA_BASE%\conf\logging.properties" goto noJuli
    set JAVA_OPTS=%JAVA_OPTS%
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
    -Djava.util.logging.config.file="%CATALINA_BASE%\conf\logging.properties"
    :noJuli

     # Start of Snippet to add
     rem Add OpenEJB javaagent if not exist
     "%CATALINA_BASE%\webapps\openejb\lib\openejb-javaagent.jar" goto
     noOpenEJBJavaagent set
     JAVA_OPTS="-javaagent:%CATALINA_BASE%\webapps\openejb\lib\openejb-javaagent.jar"
     %JAVA_OPTS% :noOpenEJBJavaagent
     # End of Snippet to add


    rem ----- Execute The Requested Command
    ---------------------------------------
    echo Using CATALINA_BASE:   %CATALINA_BASE%
    echo Using CATALINA_HOME:   %CATALINA_HOME%



    # Set juli LogManager if it is present
    if [OPENEJB: -r "$CATALINA_BASE"/conf/logging.properties ](openejb:--r-"$catalina_base"/conf/logging.properties-.html)
    ; then
    JAVA_OPTS="$JAVA_OPTS
    "-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager"
    "-Djava.util.logging.config.file="$CATALINA_BASE/conf/logging.properties"
    fi

     #Start of Snippet to add
     if [OPENEJB: -r "$CATALINA_BASE"/webapps/lib/openejb-javaagent.jar ](openejb:--r-"$catalina_base"/webapps/lib/openejb-javaagent.jar-.html)
    ; then
    JAVA_OPTS=""-javaagent:$CATALINA_BASE/lib/openejb-javaagent.jar"
    $JAVA_OPTS"
    fi
    #End of Snippet to add



##Note:
 The example above is an excerpt from the middle of the
bin/catalina.sh file. Search for the this section and add the snippet shown 
