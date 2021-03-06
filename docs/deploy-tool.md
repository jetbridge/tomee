index-group=OpenEJB Standalone Server
type=page
status=published
title=Deploy Tool
~~~~~~

<a name="DeployTool-NAME"></a>
# NAME

openejb deploy - OpenEJB Deploy Tool

<a name="DeployTool-SYNOPSIS"></a>
# SYNOPSIS

> openejb deploy [options](#DeployTool-OPTIONS) &lt;file&gt; \[&lt;file&gt; ...\]

<a name="DeployTool-NOTE"></a>
# NOTE


The OpenEJB Deploy tool is an OPTIONAL tool that allows you to deploy into
a running server and get feedback as if the app was deployed and how it was
deployed (deploymentIds, jndi names, etc.).

It can be used to deploy into an offline server, however in this scenario
it simply copies the archive into the deployment directory (by default `openejb.base/apps`) which is
something that can be done manually with a simple copy command or drag and
drop.

The OpenEJB Deploy tool can be executed from any directory as long as
`openejb.home/bin` is in the system PATH. `openejb.home` is the directory
where OpenEJB was installed or unpacked. For for the remainder of this
document we will assume you unpacked OpenEJB into the directory
`C:\openejb-3.0` under Windows.

In Windows, the deploy tool can be executed as follows:

> C:\openejb-3.0> bin\openejb deploy --help

In UNIX, Linux, or Mac OS X, the deploy tool can be executed as follows:

> user@host# bin/openejb deploy --help

Depending on your OpenEJB version, you may need to change execution bits to
make the scripts executable.  You can do this with the following command.

> user@host# chmod +x bin/openejb

From here on out, it will be assumed that you know how to execute the right
openejb script for your operating system and commands will appear in
shorthand as show below.

> openejb deploy --help


<a name="DeployTool-DESCRIPTION"></a>
# DESCRIPTION

The files passed to the Deploy Tool can be any combination of the
following:

* EJB 1.1, 2.0, 2.1, 3.0 or 3.1 jar
* application client jar 
* EAR file containing only libraries, EJBs and application clients --
everything else will be ignored.

The type of the files passed is determined as follows:

* Archives ending in `.ear` or containing a `META-INF/application.xml` are
assumed to be EAR files.
* Archives containing a `META-INF/ejb-jar.xml` file or any classes annotated
with `@Stateless`, `@Stateful` or `@MessageDriven`, are assumed to be *EJB*
applications. EJB applications older that EJB 3.0 should contain a
complete `META-INF/ejb-jar.xml` inside the jar, however we do not strictly
enforce that -- the act of it being incomplete makes it an EJB 3.0
application by nature.
* Archives containing a `META-INF/application-client.xml` or with a
`META-INF/MANIFEST.MF` containing the `Main-Class` attribute, are assumed to
be *Application Client* archives.


<a name="DeployTool-OPTIONS"></a>
# OPTIONS

<table class="mdtable">
<tr>
<td>-d, --debug </td>
<td>Increases the level of detail on validation errors and
deployment summary.</td>
</tr>

<tr><td>--dir </td>
<td>Sets the destination directory where the app will be deployed. 
The default is <OPENEJB_HOME>/apps/ directory.	Note when changing this
setting make sure the directory is listed in the openejb.xml via a
<Deployments dir=""/> tag or the app will not be picked up again on
restart.
</tr>

<tr><td>-conf file </td>
<td>Sets the OpenEJB configuration to the specified file.</td></tr>

<tr><td>-h, --help </td>
<td>Lists these options and exit.</td></tr>

<tr><td>-o, --offline</td>
<td>Deploys the app to an offline server by copying the
archive into the server's apps/ directory.  The app will be deployed when
the server is started.	The default is online.</td></tr>

<tr><td>-q, --quiet	</td>
<td> Decreases the level of detail on validation and skips the
deployment summary.</td></tr>

<tr><td>-s, --server-url &lt;url&gt; </td>
<td>   Sets the url of the OpenEJB server to which
the app will be deployed.  The value should be the same as the JNDI
Provider URL used to lookup EJBs.  The default is 'ejbd://localhost:4201'.
</td></tr>

<tr><td>-v, --version</td>
<td>   Prints the OpenEJB version and exits. </td></tr>
</table>


<a name="DeployTool-EXAMPLES"></a>
# EXAMPLES


<a name="DeployTool-Deployingmultiplejarfiles"></a>
## Deploying multiple jar files


> openejb deploy myapp\fooEjbs.jar myapp\barEjbs.jar


Deploys the beans in the fooEjbs.jar first, then deploys the beans in the
barEjbs.jar. Wildcards can be used as well.

> openejb deploy myapp\*.jar


<a name="DeployTool-OUTPUT"></a>
# OUTPUT

On running the deploy tool with a valid EJB jar the following output is
printed on the console


    Application deployed successfully at {0}
    App(id=C:\samples\Calculator-new\hello-addservice.jar)
        EjbJar(id=hello-addservice.jar, path=C:\samples\Calculator-new\hello-addservice.jar)
    	Ejb(ejb-name=HelloBean, id=HelloBean)
    	    Jndi(name=HelloBean)
    	    Jndi(name=HelloBeanLocal)
    
    	Ejb(ejb-name=AddServiceBean, id=AddServiceBean)
    	    Jndi(name=AddServiceBean)
    	    Jndi(name=AddServiceBeanLocal)


Note: In the above case the command used is:
> openejb deploy hello-addservice.jar

The JAR file contains two EJBs: AddServiceBean and HelloBean.
