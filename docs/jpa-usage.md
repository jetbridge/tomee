index-group=JPA
type=page
status=published
title=JPA Usage
~~~~~~
<a name="JPAUsage-Thingstowatchoutfor"></a>
# Things to watch out for

<a name="JPAUsage-Critical:Alwayssetjta-data-sourceandnon-jta-data-source"></a>
## Critical: Always set jta-data-source and non-jta-data-source

Always set the value of jta-data-source and non-jta-data-source in your
persistence.xml file.  Regardless if targeting your EntityManager usage for
transaction-type="RESOURCE_LOCAL" or transaction-type="TRANSACTION", it's
very difficult to guarantee one or the other will be the only one needed. 
Often times the JPA Provider itself will require both internally to do
various optimizations or other special features.  

* The *jta-data-source* should always have it's OpenEJB-specific
'*JtaManaged*' property set to '*true*'  (this is the default)
* The *non-jta-data-source* should always have it's OpenEJB-specific
'*JtaManaged*' property set to '*false*'.

See [Containers and Resources](containers-and-resources.html)
 for how to configure 'JtaManaged' and a full list of <Resource> properties
for DataSources.

<a name="JPAUsage-Bedetachaware"></a>
## Be detach aware

A warning for any new JPA user is by default all objects will detach at the
end of a transaction.  People typically discover this when the go to remove
or update an object they fetched previously and get an exception like "You
cannot perform operation delete on detached object".

All ejb methods start a transaction unless a) you [configure them otherwise](transaction-annotations.html)
, or b) the caller already has a transaction in progress when it calls the
bean.  If you're in a test case or a servlet, it's most likely B that is
biting you.  You're asking an ejb for some persistent objects, it uses the
EntityManager in the scope of the transaction started around it's method
and returns some persistent objects, by the time you get them the
transaction has completed and now the objects are detached.

<a name="JPAUsage-Solutions"></a>
### Solutions
1. Call EntityManager.merge(..) inside the bean code to reattach your
object.
1. Use PersistenceContextType.EXTENDED as in '@PersistenceContext(unitName
= "movie-unit", type = PersistenceContextType.EXTENDED)' for EntityManager
refs instead of the default of PersistenceContextType.TRANSACTION.
1. If testing, use a technique to execute transactions in your test code. 
That's described here in [Unit testing transactions](unit-testing-transactions.html)
