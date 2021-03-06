index-group=Unrevised
type=page
status=published
title=Property Overriding
~~~~~~
OpenEJB consists of several components (containers, resource adapters,
security services, etc.) all of which are pluggable and have their own
unique set of configurable properties.	These components are required to
specify their default property values in their service-jar.xml file.  This
means that at a minimum you as a user only need to specify what you'd like
to be different.  You specify the property name and the new value and the
default value will be overwritten before the component is created.

We call this overriding and there are several ways to do it.

<a name="PropertyOverriding-Theopenejb.xml"></a>
#  The openejb.xml

The default openejb.xml file has most of the useful properties for each
component explicitly listed with default values for documentation purposes.
 It is safe to delete them and be assured that no behavior will change if a
smaller config file is desired.


Overriding can also be done via the command line or plain Java system
properties.  See [System Properties](system-properties.html)
 for details.

<a name="PropertyOverriding-Whatpropertiesareavailable?"></a>
## What properties are available?
    
To know what properties can be overriden the './bin/openejb properties'
command is very useful: see [Properties Tool](properties-tool.html)
    
It's function is to connect to a running server and print a canonical list
of all properties OpenEJB can see via the various means of configuration. 
When sending requests for help to the users list or jira, it is highly
encouraged to send the output of this tool with your message.

<a name="PropertyOverriding-Notconfigurableviaopenejb.xml"></a>
## Not configurable via openejb.xml
    
The only thing not yet configurable via this file are ServerServices due to
OpenEJB's embeddable nature and resulting long standing tradition of
keeping the container system separate from the server layer.  This may
change someday, but untill then ServerServices are configurable via
conf/<service-id>.properties files such as conf/ejbd.properties to
configure the main protocol that services EJB client requests.

The format those properties files is greatly adapted from the xinet.d style
of configuration and even shares similar functionality and properties such
as host-based authorization (HBA) via the 'only_from' property.

<a name="PropertyOverriding-Restoringopenejb.xmltothedefaults"></a>
## Restoring openejb.xml to the defaults

To restore this file to its original default state, you can simply delete
it or rename it and OpenEJB will see it's missing and unpack another
openejb.xml into the conf/ directory when it starts.

This is not only handy for recovering from a non-functional config, but
also for upgrading as OpenEJB will not overwrite your existing
configuration file should you choose to unpack an new distro over the top
of an old one -- this style of upgrade is safe provided you move your old
lib/ directory first.
