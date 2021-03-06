index-group=Unrevised
type=page
status=published
title=Validation Tool
~~~~~~

<a name="ValidationTool-NAME"></a>
# NAME

openejb validate - OpenEJB Validation Tool

<a name="ValidationTool-SYNOPSIS"></a>
# SYNOPSIS


openejb validate [options](options.html)
 jarfiles

<a name="ValidationTool-NOTE"></a>
# NOTE


The OpenEJB Validation tool must be executed from the OPENEJB_HOME
directory. This is the directory where OpenEJB was installed or unpacked.
For for the remainder of this document we will assume you unpacked OpenEJB
into the directory C:\openejb.

In Windows, the validation tool can be executed as follows:

*C:\openejb> openejb validate -help*


{warning}
There is a bug in the openejb.bat script of OpenEJB 0.9.0 that doesn't
allow you to execute the validate command as above. For that release,
Windows users can execute the following command:
*C:\openejb> bin\validate.bat -help*

{warning}


In UNIX, Linux, or Mac OS X, the deploy tool can be executed as follows:

`[user@host openejb](user@host-openejb.html)
# ./openejb.sh validate -help`


Depending on your OpenEJB version, you may need to change execution bits to
make the scripts executable. You can do this with the following command.

`[user@host openejb](user@host-openejb.html)
# chmod 755 openejb.sh bin/*.sh`


From here on out, it will be assumed that you know how to execute the right
openejb script for your operating system and commands will appear in
shorthand as show below.

*openejb validate -help*


<a name="ValidationTool-DESCRIPTION"></a>
# DESCRIPTION


The validation tool currently checks for the following things:
* Does the Jar have all the ejb-class class files
* Does the Jar have all the home class files
* Does the Jar have all the remote class files
* Is the ejb-class a sub-interface of SessionBean or EntityBean
* Is the home a sub-interface of EJBHome
* Is the remote a sub-interface of EJBObject
* Are all the methods in the remote interface implemented in the ejb-class
* Are there create methods in the home interface
* Are the create methods in the home interface implemented in the ejb-class
* Are the required post create methods in the ejb-class of EntityBeans
* Are there any create methods in the ejb-class, but not in the home
interface

More checks will be added in the future.

<a name="ValidationTool-OPTIONS"></a>
# OPTIONS


<table class="mdtable">
<tr><td>-v</td><td>Sets the output level to 1. This will output just the minumum details
on each failure.</td></tr>
<tr><td>-vv</td><td>Default. Sets the output level to 2. Outputs one line summaries of
each failure. This is the default output level.</td></tr>
<tr><td>-vvv</td><td>Sets the output level to 3. Outputs verbose details on each failure,
usually with details on how to correct the failures.</td></tr>
<tr><td>-xml</td><td>Outputs information in well-formed XML.</td></tr>
<tr><td>-nowarn</td><td>Suppresses warnings.</td></tr>
<tr><td>-version</td><td>Print the version.</td></tr>
<tr><td>-help</td><td>Print this help message.</td></tr>
<tr><td>-examples</td><td>Show examples of how to use the options.</td></tr>
</table>

<a name="ValidationTool-COMMONISSUES"></a>
# COMMON ISSUES

<a name="ValidationTool-MisslocatedclassorNoClassDefFoundError"></a>
## Misslocated class or NoClassDefFoundError

The short explanation is that the parent doesn't have all the classes it
needs as some of them are only in the child classloader, where the parent
can't see them.

This would occur, for example, if a class was loaded by the parent
classloader, but that class' superclass wasn't visible to the parent
classloader, perhaps because it is only in the child classloader.

Here is a more concrete example:

    public interface Person extends EJBObject {
    }
    
    public interface Employee extends Person {
    }


Ok, so when we build our ejb jar, we put both the Person and Employee
interfaces in the jar, so everything should be good (so we think). But now
let's say that for some reason the Employee interface is also in another
jar and that jar was loaded into the system classpath.

When a new classloader is create for my ejb-jar at runtime and the system
attempts to load the Employee interface, the call goes right through that
classloader and down to the system classloader. The Employee interface is
found, because it was accidentally added to that extra jar in the system
classpath. So now the system classloader goes looking for Employee's
superinterface, Person, where it immediatly blows up and throws a
NoClassDefFoundError: Person.

Most people will look at their ejb-jar and think, "But all my classes are
there!?", which is true. It really doesn't matter though, because one of
those classes is also in the parent classloader. The first call to load
that class will bypass your classloader completely and go to the parent.
Once there, it is the parent's job to find *all* the dependent classes. If
it can't ... NoClassDefFoundError.
