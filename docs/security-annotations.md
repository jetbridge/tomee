index-group=Unrevised
type=page
status=published
title=Security Annotations
~~~~~~
This page shows the correct usage of the security related annotations:

 - javax.annotation.security.RolesAllowed
 - javax.annotation.security.PermitAll
 - javax.annotation.security.DenyAll
 - javax.annotation.security.RunAs
 - javax.annotation.security.DeclareRoles

<a name="SecurityAnnotations-Basicidea"></a>
## Basic idea

- By default all methods of a business interface are accessible, logged in
or not
- The annotations go on the bean class, not the business interface
- Security annotations can be applied to entire class and/or individual
methods
- The names of any security roles used must be declared via @DeclareRoles

<a name="SecurityAnnotations-Norestrictions"></a>
## No restrictions

Allow anyone logged in or not to invoke 'svnCheckout'.

These three examples are all equivalent.


    @Stateless
    public class OpenSourceProjectBean implements Project {
    
        public String svnCheckout(String s) {
    	return s;
        }
    }


    @Stateless
    @PermitAll
    public class OpenSourceProjectBean implements Project {
    
        public String svnCheckout(String s) {
    	return s;
        }
    }


    @Stateless
    public class OpenSourceProjectBean implements Project {
    
        @PermitAll
        public String svnCheckout(String s) {
    	return s;
        }
    }


 - Allow anyone logged in or not to invoke 'svnCheckout'.

<a name="SecurityAnnotations-RestrictingaMethod"></a>
## Restricting a Method

Restrict the 'svnCommit' method to only individuals logged in and part of
the "committer" role.  Note that more than one role can be listed.


    @Stateless
    @DeclareRoles({"committer"})
    public class OpenSourceProjectBean implements Project {
    
        @RolesAllowed({"committer"})
        public String svnCommit(String s) {
    	return s;
        }
    
        public String svnCheckout(String s) {
    	return s;
        }
    }


 - Allow only logged in users in the "committer" role to invoke
'svnCommit'.
 - Allow anyone logged in or not to invoke 'svnCheckout'.


<a name="SecurityAnnotations-DeclareRoles"></a>
## DeclareRoles

You need to update the @DeclareRoles when referencing roles via
isCallerInRole(roleName).


    @Stateless
    @DeclareRoles({"committer", "contributor"})
    public class OpenSourceProjectBean implements Project {
    
        @Resource SessionContext ctx;
    
        @RolesAllowed({"committer"})
        public String svnCommit(String s) {
    	ctx.isCallerInRole("committer"); // Referencing a Role
    	return s;
        }
    
        @RolesAllowed({"contributor"})
        public String submitPatch(String s) {
    	return s;
        }
    }


<a name="SecurityAnnotations-Restrictingallmethodsinaclass"></a>
##  Restricting all methods in a class

Placing the annotation at the class level changes the default of PermitAll


    @Stateless
    @DeclareRoles({"committer"})
    @RolesAllowed({"committer"})
    public class OpenSourceProjectBean implements Project {
    
        public String svnCommit(String s) {
    	return s;
        }
    
        public String svnCheckout(String s) {
    	return s;
        }
    
        public String submitPatch(String s) {
    	return s;
        }
    }


- Allow only logged in users in the "committer" role to invoke 'svnCommit',
'svnCheckout' or 'submitPatch'.

<a name="SecurityAnnotations-Mixingclassandmethodlevelrestrictions"></a>
##  Mixing class and method level restrictions

Security annotations can be used at the class level and method level at the
same time.  These rules do not stack, so marking 'submitPatch' overrides
the default of "committers".


    @Stateless
    @DeclareRoles({"committer", "contributor"})
    @RolesAllowed({"committer"})
    public class OpenSourceProjectBean implements Project {
    
        public String svnCommit(String s) {
    	return s;
        }
    
        public String svnCheckout(String s) {
    	return s;
        }
    
        @RolesAllowed({"contributor"})
        public String submitPatch(String s) {
    	return s;
        }
    }


 - Allow only logged in users in the "committer" role to invoke 'svnCommit'
or 'svnCheckout'
 - Allow only logged in users in the "contributor" role to invoke
'submitPatch'.	

<a name="SecurityAnnotations-PermitAll"></a>
##  PermitAll

When annotating a bean class with @RolesAllowed, the @PermitAll annotation
becomes very useful on individual methods to open them back up again.


    @Stateless
    @DeclareRoles({"committer", "contributor"})
    @RolesAllowed({"committer"})
    public class OpenSourceProjectBean implements Project {
    
        public String svnCommit(String s) {
    	return s;
        }
    
        @PermitAll
        public String svnCheckout(String s) {
    	return s;
        }
    
        @RolesAllowed({"contributor"})
        public String submitPatch(String s) {
    	return s;
        }
    }


 - Allow only logged in users in the "committer" role to invoke
'svnCommit'.
 - Allow only logged in users in the "contributor" role to invoke
'submitPatch'.
 - Allow anyone logged in or not to invoke 'svnCheckout'.


<a name="SecurityAnnotations-DenyAll"></a>
##  DenyAll

The @DenyAll annotation can be used to restrict business interface access
from anyone, logged in or not.	The method is still invokable from within
the bean class itself.


    @Stateless
    @DeclareRoles({"committer", "contributor"})
    @RolesAllowed({"committer"})
    public class OpenSourceProjectBean implements Project {
    
        public String svnCommit(String s) {
    	return s;
        }
    
        @PermitAll
        public String svnCheckout(String s) {
    	return s;
        }
    
        @RolesAllowed({"contributor"})
        public String submitPatch(String s) {
    	return s;
        }
    
        @DenyAll
        public String deleteProject(String s) {
    	return s;
        }
    }


 - Allow only logged in users in the "committer" role to invoke
'svnCommit'.
 - Allow only logged in users in the "contributor" role to invoke
'submitPatch'.
 - Allow anyone logged in or not to invoke 'svnCheckout'.
 - Allow *no one* logged in or not to invoke 'deleteProject'.

<a name="SecurityAnnotations-IllegalUsage"></a>
#  Illegal Usage

Generally, security restrictions cannot be made on AroundInvoke methods and
most callbacks.

The following usages of @RolesAllowed have no effect.


    @Stateful
    @DecalredRoles({"committer"})
    public class MyStatefulBean implements	MyBusinessInterface  {
    
        @PostConstruct
        @RolesAllowed({"committer"})
        public void constructed(){
    
        }
    
        @PreDestroy
        @RolesAllowed({"committer"})
        public void destroy(){
    
        }
    
        @AroundInvoke
        @RolesAllowed({"committer"})
        public Object invoke(InvocationContext invocationContext) throws
Exception {
    	return invocationContext.proceed();
        }
    
        @PostActivate
        @RolesAllowed({"committer"})
        public void activated(){
    
        }
    
        @PrePassivate
        @RolesAllowed({"committer"})
        public void passivate(){
    
        }
    }
