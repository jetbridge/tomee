index-group=Configuration
type=page
status=published
title=Security
~~~~~~
<a name="Security-Security-HowTo."></a>
# Security - How To.

We currently have two authentication mechanisms to choose from:
* *PropertiesLoginModule* (a basic text file based login that looks up
users and groups from the specified properties files)
* *SQLLoginModule* (database based login that looks up users and groups
in a database through SQL queries)

To make your program authenticate itself to the server, simply construct
your InitialContext with the standard javax.naming.Context properties for
user/pass info, which is:

    Properties props = new Properties();
    props.setProperty(Context.INITIAL_CONTEXT_FACTORY, "org.apache.openejb.client.RemoteInitialContextFactory");
    props.setProperty(Context.PROVIDER_URL, "ejbd://localhost:4201");
    props.setProperty(Context.SECURITY_PRINCIPAL, "someuser");
    props.setProperty(Context.SECURITY_CREDENTIALS, "thepass");
    props.setProperty("openejb.authentication.realmName", "PropertiesLogin");
    // optional
    InitialContext ctx = new InitialContext(props);
    ctx.lookup(...);

That will get you logged in and all your calls from that context should
execute as you.

*$\{openejb.base\}/conf/login.config* is a standard JAAS config file.
Here, you can configure any number of security realms to authenticate
against.
To specify which of the realms you want to authenticate against, you can
set the *openejb.authentication.realmName* property to any of the
configured realm names in *login.config*.
If you don't speficy a realm name, the default (currently
*PropertiesLogin*) is used.
For examples and more information on JAAS configuration, see the [JAAS Reference Guide](http://java.sun.com/javase/6/docs/technotes/guides/security/jaas/JAASRefGuide.html)
.

<a name="Security-PropertiesLoginModule"></a>
## PropertiesLoginModule

Supported options:
<table class="mdtable">
<tr><th>Option</th><th>Description</th><th>Required</th></tr>
<tr><td>UsersFile</td><td>name of the properties file that contains the users and their
passwords</td><td>*yes*</td></tr>
<tr><td>GroupsFile</td><td>name of the properties file that contains the groups and their
member lists</td><td>*yes*</td></tr>
</table>

*UsersFile* and *GroupsFile* are read in on every login, so +you can
update them+ on a running system and those users will "show up" immediately
+without the need for a restart+ of any kind.

<a name="Security-SQLLoginModule"></a>
## SQLLoginModule

You can either use a data source or configure the JDBC URL through which
the user/group lookups will be made.

If you use a *DataSource*, you must specify its JNDI name with the
*dataSourceName* option.

If you use JDBC directly, you have to specify at least the JDBC URL of the
database.
The driver should be autodetected (provided the appropriate jar is on your
classpath), but if that fails for some reason, you can force a specific
driver using the *jdbcDriver* option.
For more information on JDBC URLs, see the [JDBC Guide](http://java.sun.com/javase/6/docs/technotes/guides/jdbc/)

The *userSelect* query must return a two-column list of user names
(column 1) and passwords (column 2). This query should normally return a
single row, which can be achieved by the use of a query parameter
placeholder "?".
Any such placeholders in both queries will be filled in with the username
that the client is trying to log in with.
The *groupSelect* query must return a two-column list of user names and
their groups (or "roles" in the EJB world).

Supported options:
<table class="mdtable">
<tr><th>Option</th><th>Description</th><th>Required</th></tr>
<tr><td>dataSourceName</td><td>the name of a data source</td><td>*yes* (alternative 1)</td></tr>
<tr><td>jdbcURL</td><td>a standard JDBC URL</td><td>*yes* (alternative 2)</td></tr>
<tr><td>jdbcDriver</td><td>the fully qualified class name of the database driver</td><td>no</td></tr>
<tr><td>jdbcUser</td><td>the user name for accessing the database</td><td>no</td></tr>
<tr><td>jdbcPassword</td><td>the password for accessing the database</td><td>no</td></tr>
<tr><td>userSelect</td><td>the SQL query that returns a list of users and their
passwords</td><td>*yes*
</tr>
<tr><td>groupSelect</td><td>the SQL query that returns a list of users and groups
(roles)</td><td>*yes*
</tr>
<tr><td>digest</td><td>the name of the digest algorithm (e.g. "MD5" or "SHA") for digest
authentication</td><td>no</td></tr>
<tr><td>encoding</td><td>the digest encoding, can be "hex" or "base64"</td><td>no</td></tr>
</table>

<a name="Security-PLUGPOINTS"></a>
# PLUG POINTS

There are four-five different plug points where you could customize the
functionality.	From largest to smallest:
- *The SecurityService interface*:  As before all security work
(authentication and authorization) is behind this interface, only the
methods on it have been updated.  If you want to do something really "out
there" or need total control, this is where you go. Plugging in your own
SecurityService should really be a last resort. We still have our "do
nothing" SecurityService implementation just as before, but it is no longer
the default. +You can add a new SecurityService impl by creating a
service-jar.xml and packing it in your jar+.  You can configure OpenEJB to
use a different SecurityService via the openejb.xml.

- *JaccProvider super class*:  If you want to plug in your own JACC
implementation to perform custom authorization (maybe do some fancy
auditing), this is one way to do it without really having to understand
JACC too much.	We will plug your provider in to all the places required by
JACC if you simply +set the system property+
"*org.apache.openejb.core.security.JaccProvider*" with the name of your
JaccProvider impl.

- *Regular JACC*.  The JaccProvider is simply a wrapper around the many
things you have to do to create and plugin a JACC provider, but you can
still plugin a JACC provider in the standard ways.  Read the JACC spec for
that info.

- *JAAS LoginModule*.  You can setup a different JAAS LoginModule to do all
your authentication by simply editing the conf/login.config file which is a
plain JAAS config file.  At the moment we only support username/password
based login modules.  At some point it would be nice to support any kind of
input for a JAAS LoginModule, but username/password at least covers the
majority.  It actually *is* possible to support any LoginModule, but you
would have to supply your clients with your own way to authenticate to it
and write a strategy for telling the OpenEJB client what data to send to
the server with each invocation request. See the [JAAS LoginModule Developer's Guide](http://java.sun.com/javase/6/docs/technotes/guides/security/jaas/JAASLMDevGuide.html)
 for more information.

- *Client IdentityResolver*.  This is the just mentioned interface you
would have to implement to supply the OpenEJB client with alternate data to
send to the server with each invocation request. If you're plugging in a
new version of this it is likely that you may also want to plugin in your
own SecurityService implementation. Reason being, the object returned from
IdentiyResolve.getIdentity() is sent across the wire and straight in to the
SecurityService.associate(Object) method.
