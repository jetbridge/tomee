index-group=Unrevised
type=page
status=published
title=App Clients and JNDI
~~~~~~
There are some slight differences between the way OpenEJB does app clients
and the way Geronimo does app clients


Neither uses the names created via the openejb.jndiname.format. &nbsp;So
changing that will (should) have no affect. &nbsp;The idea is that users
should be able to set it to be whatever they want it to be and that should
not break the App Client code. &nbsp;The openejb.jndiname.format is
specifically for "plain" clients and allows them to get the names as they
want them.

Internally, we bind each EJB proxy under essentially a hardcoded and
predictable format and then again using the user supplied format. &nbsp;So
there are at minimum two JNDI trees with every EJB proxy. &nbsp;It used to
be two at least. &nbsp;Now we have quite a few because of Java EE 6 global
JNDI and the support we added for @LocalClient and allowing the same
interface to be used as both @Local and @Remote.

Basically we have:

* openejb/Deployment/&lt;hardcoded internal format&gt;
* openejb/local/&lt;strategy format&gt;
* openejb/remote/&lt;strategy format&gt;

The 'openejb/Deployment' section is the non-changing fully qualified name
for use internally and by app clients.

The 'openejb/remote' section is for "pretty" names looked up via plain
clients using the RemoteInitialContextFactory. &nbsp;The protocol can tell
the difference between app clients and plain clients and knows which area
to look in.

The 'openejb/local' section is for "pretty" names looked up via the
LocalInitialContextFactory.

The "pretty" names are defined by the openejb.jndiname.format and since the
user has control of that formatting it's possible that not all proxies can
be bound. &nbsp;Say the bean has both a local and remote interface and the
user has just "\{deploymentId\}" or "\{ejbName\}" as the format.
&nbsp;Hence those bind calls use the "optional" set of binding methods.

The format of the internal names bound into openejb/Deployment is
guaranteed to be unique. &nbsp;It's not pretty to look at obviously, but
every possible proxy will be bound there guaranteed. &nbsp;For binding into
'openejb/Deployment' we don't use the "optional" set of binding methods.
&nbsp;If something can't be bound it's a deployment issue.

The home interface is bound, but with the name of the corresponding
business interface rather than the home interface. &nbsp;

To be a little bit more clear - &nbsp;Both OpenEJB and Geronimo build their
own JNDI trees for the App Client. &nbsp;Geronimo prefers to have its own
JNDI tree for the App Client as there are other things in it that are not
EJB related. &nbsp;Either way the OpenEJB EJBd protocol can carry the "id"
of the App Client and both Geronimo and OpenEJB rely on that.

In Geronimo App Clients the id is set to "Deployments" and that tells
OpenEJB not to look in the "openejb/remote" section of JNDI as it normally
would. &nbsp;It will instead use the "openejb/Deployments" section of JNDI
were the names follow a predictable and unchanging format.

In OpenEJB App Clients the id is set to the name of the App Client and we
instead look in "openejb/client/<id>/" where names are formatted by the
user via the application-client.xml.

When calls are made from client to server and the App Client module id is
not present, we look in openejb/remote/ where names are formatted using the
openejb.jndi.format
