index-group=Datasource
type=page
status=published
title=DataSource Configuration
~~~~~~

A DataSource can be declared via xml in the `<tomee-home>/conf/tomee.xml` file or in a `WEB-INF/resources.xml` file using a declaration like the following.  All properties in the element body are optional.

    <Resource id="myDataSource" type="javax.sql.DataSource">
        accessToUnderlyingConnectionAllowed = false
        alternateUsernameAllowed = false
        connectionProperties = 
        defaultAutoCommit = true
        defaultReadOnly = 
        definition = 
        ignoreDefaultValues = false
        initialSize = 0
        jdbcDriver = org.hsqldb.jdbcDriver
        jdbcUrl = jdbc:hsqldb:mem:hsqldb
        jtaManaged = true
        maxActive = 20
        maxIdle = 20
        maxOpenPreparedStatements = 0
        maxWaitTime = -1 millisecond
        minEvictableIdleTime = 30 minutes
        minIdle = 0
        numTestsPerEvictionRun = 3
        password = 
        passwordCipher = PlainText
        poolPreparedStatements = false
        serviceId = 
        testOnBorrow = true
        testOnReturn = false
        testWhileIdle = false
        timeBetweenEvictionRuns = -1 millisecond
        userName = sa
        validationQuery = 
    </Resource>

Alternatively, a DataSource can be declared via properties in the `<tomee-home>/conf/system.properties` file or via Java VirtualMachine `-D` properties.  The properties can also be used when embedding TomEE via the `javax.ejb.embeddable.EJBContainer` API or `InitialContext`

    myDataSource = new://Resource?type=javax.sql.DataSource
    myDataSource.accessToUnderlyingConnectionAllowed = false
    myDataSource.alternateUsernameAllowed = false
    myDataSource.connectionProperties = 
    myDataSource.defaultAutoCommit = true
    myDataSource.defaultReadOnly = 
    myDataSource.definition = 
    myDataSource.ignoreDefaultValues = false
    myDataSource.initialSize = 0
    myDataSource.jdbcDriver = org.hsqldb.jdbcDriver
    myDataSource.jdbcUrl = jdbc:hsqldb:mem:hsqldb
    myDataSource.jtaManaged = true
    myDataSource.maxActive = 20
    myDataSource.maxIdle = 20
    myDataSource.maxOpenPreparedStatements = 0
    myDataSource.maxWaitTime = -1 millisecond
    myDataSource.minEvictableIdleTime = 30 minutes
    myDataSource.minIdle = 0
    myDataSource.numTestsPerEvictionRun = 3
    myDataSource.password = 
    myDataSource.passwordCipher = PlainText
    myDataSource.poolPreparedStatements = false
    myDataSource.serviceId = 
    myDataSource.testOnBorrow = true
    myDataSource.testOnReturn = false
    myDataSource.testWhileIdle = false
    myDataSource.timeBetweenEvictionRuns = -1 millisecond
    myDataSource.userName = sa
    myDataSource.validationQuery = 

Properties and xml can be mixed.  Properties will override the xml allowing for easy configuration change without the need for ${} style variable substitution.  Properties are not case sensitive.  If a property is specified that is not supported by the declared DataSource a warning will be logged.  If a DataSource is needed by the application and one is not declared, TomEE will create one dynamically using default settings.  Multiple DataSource declarations are allowed.

See the [Common DataSource Configurations](common-datasource-configurations.html) page for examples of configuring datasources for Derby, MySQL, Oracle and other common databases.

# Supported Properties
<table class="mdtable">
<tr>
<th>Property</th>
<th>Type</th>
<th>Default</th>
<th>Description</th>
</tr>
<tr>
  <td><a href="#accessToUnderlyingConnectionAllowed">accessToUnderlyingConnectionAllowed</a></td>
  <td>boolean</td>
  <td>false</td>
  <td>
If true the raw physical connection to the database can be
accessed
</td>
</tr>
</tr>
<tr>
  <td><a href="#alternateUsernameAllowed">alternateUsernameAllowed</a></td>
  <td>boolean</td>
  <td>false</td>
  <td>
If true allow an alternate username and password to be specified on the connection, rather than those specified in the DataSource definition..
</td>
</tr>
<tr>
  <td><a href="#connectionProperties">connectionProperties</a></td>
  <td>String</td>
  <td></td>
  <td>
The connection properties that will be sent to the JDBC
driver when establishing new connections
</td>
</tr>
<tr>
  <td>defaultAutoCommit</td>
  <td>boolean</td>
  <td>true</td>
  <td>
The default auto-commit state of new connections
</td>
</tr>
<tr>
  <td>defaultReadOnly</td>
  <td>String</td>
  <td></td>
  <td>
The default read-only state of new connections
If not set then the setReadOnly method will not be called.
(Some drivers don't support read only mode, ex: Informix)
</td>
</tr>
<tr>
  <td>definition</td>
  <td>String</td>
  <td></td>
  <td>

</td>
</tr>
<tr>
  <td>ignoreDefaultValues</td>
  <td>boolean</td>
  <td>false</td>
  <td>
use only all set values in this config
will need a lot of properties but allow to not set some values
</td>
</tr>
<tr>
  <td><a href="#initialSize">initialSize</a></td>
  <td>int</td>
  <td>0</td>
  <td>
The size to reach when creating the datasource.
</td>
</tr>
<tr>
  <td>jdbcDriver</td>
  <td>String</td>
  <td>org.hsqldb.jdbcDriver</td>
  <td>
Driver class name
</td>
</tr>
<tr>
  <td>jdbcUrl</td>
  <td>java.net.URI</td>
  <td>jdbc:hsqldb:mem:hsqldb</td>
  <td>
Url for creating connections
</td>
</tr>
<tr>
  <td><a href="#jtaManaged">jtaManaged</a></td>
  <td>boolean</td>
  <td>true</td>
  <td>
Determines wether or not this data source should be JTA managed
or user managed.
</td>
</tr>
<tr>
  <td>maxActive</td>
  <td>int</td>
  <td>20</td>
  <td>
The maximum number of active connections that can be
allocated from this pool at the same time, or a negative
number for no limit. N.B. When using dbcp2 with TomEE 7 ("DataSourceCreator dbcp"), "MaxTotal" should be used as opposed to "MaxActive".
</td>
</tr>
<tr>
  <td>maxIdle</td>
  <td>int</td>
  <td>20</td>
  <td>
The maximum number of connections that can remain idle in
the pool, without extra ones being released, or a negative
number for no limit.
</td>
</tr>
<tr>
  <td><a href="#maxOpenPreparedStatements">maxOpenPreparedStatements</a></td>
  <td>int</td>
  <td>0</td>
  <td>
The maximum number of open statements that can be allocated
from the statement pool at the same time, or zero for no
limit.
</td>
</tr>
<tr>
  <td>maxWaitTime</td>
  <td><a href="configuring-durations.html">time</a></td>
  <td>-1&nbsp;millisecond</td>
  <td>
The maximum number of time that the pool will wait
(when there are no available connections) for a connection
to be returned before throwing an exception, or -1 to wait
indefinitely.
</td>
</tr>
<tr>
  <td>minEvictableIdleTime</td>
  <td><a href="configuring-durations.html">time</a></td>
  <td>30&nbsp;minutes</td>
  <td>
The minimum amount of time a connection may sit idle in the
pool before it is eligable for eviction by the idle
connection evictor (if any).
</td>
</tr>
<tr>
  <td>minIdle</td>
  <td>int</td>
  <td>0</td>
  <td>
The minimum number of connections that can remain idle in
the pool, without extra ones being created, or zero to
create none.
</td>
</tr>
<tr>
  <td>numTestsPerEvictionRun</td>
  <td>int</td>
  <td>3</td>
  <td>
The number of connectionss to examine during each run of the
idle connection evictor thread (if any).
</td>
</tr>
<tr>
  <td>password</td>
  <td>String</td>
  <td></td>
  <td>
Default password
</td>
</tr>
<tr>
  <td>passwordCipher</td>
  <td>String</td>
  <td>PlainText</td>
  <td>

</td>
</tr>
<tr>
  <td><a href="#poolPreparedStatements">poolPreparedStatements</a></td>
  <td>boolean</td>
  <td>false</td>
  <td>
If true, a statement pool is created for each Connection and
PreparedStatements created by one of the following methods are
pooled:
</td>
</tr>
<tr>
  <td>serviceId</td>
  <td>String</td>
  <td></td>
  <td>

</td>
</tr>
<tr>
  <td><a href="#testOnBorrow">testOnBorrow</a></td>
  <td>boolean</td>
  <td>true</td>
  <td>
If true connections will be validated before being returned
from the pool. If the validation fails, the connection is
destroyed, and a new conection will be retrieved from the
pool (and validated).
</td>
</tr>
<tr>
  <td><a href="#testOnReturn">testOnReturn</a></td>
  <td>boolean</td>
  <td>false</td>
  <td>
If true connections will be validated before being returned
to the pool.  If the validation fails, the connection is
destroyed instead of being returned to the pool.
</td>
</tr>
<tr>
  <td><a href="#testWhileIdle">testWhileIdle</a></td>
  <td>boolean</td>
  <td>false</td>
  <td>
If true connections will be validated by the idle connection
evictor (if any). If the validation fails, the connection is
destroyed and removed from the pool
</td>
</tr>
<tr>
  <td>timeBetweenEvictionRuns</td>
  <td><a href="configuring-durations.html">time</a></td>
  <td>-1&nbsp;millisecond</td>
  <td>
The number of milliseconds to sleep between runs of the idle
connection evictor thread. When set to a negative number, no
idle connection evictor thread will be run.
</td>
</tr>
<tr>
  <td>userName</td>
  <td>String</td>
  <td>sa</td>
  <td>
Default user name
</td>
</tr>
<tr>
  <td>validationQuery</td>
  <td>String</td>
  <td></td>
  <td>
The SQL query that will be used to validate connections from
this pool before returning them to the caller. If specified,
this query MUST be an SQL SELECT statement that returns at
least one row.
</td>
</tr>
<tr>
<td>LogSql</td>
<td>boolean</td>
<td>false</td>
<td>Wether SQL queries should be logged or not</td>
</tr>
</table>



<a name="accessToUnderlyingConnectionAllowed"></a>
## accessToUnderlyingConnectionAllowed

If true the raw physical connection to the database can be
accessed using the following construct:

    Connection conn = ds.getConnection();
    Connection rawConn = ((DelegatingConnection) conn).getInnermostDelegate();
    ...
    conn.close()

Default is false, because misbehaving programs can do harmfull
things to the raw connection shuch as closing the raw
connection or continuing to use the raw connection after it
has been assigned to another logical connection.  Be careful
and only use when you need direct access to driver specific
extensions.

NOTE: Do NOT close the underlying connection, only the
original logical connection wrapper.


<a name="connectionProperties"></a>
## connectionProperties

The connection properties that will be sent to the JDBC
driver when establishing new connections

Format of the string must be [propertyName=property;]*

NOTE - The "user" and "password" properties will be passed
explicitly, so they do not need to be included here.


<a name="TransactionIsolation"></a>
## TransactionIsolation


The default TransactionIsolation state of new connections.

If not set then the `setTransactionIsolation` method will not
be called. The allowed values for this property are:

- `NONE`
- `READ_COMMITTED`
- `READ_UNCOMMITTED`
- `REPEATABLE_READ`
- `SERIALIZABLE`

Note: Most JDBC drivers do not support all isolation levels
DefaultTransactionIsolation

<a name="initialSize"></a>
## initialSize

The initial size to initialize the pool of connections.

<a name="jtaManaged"></a>
## jtaManaged

Determines wether or not this data source should be JTA managed
or user managed.

If set to 'true' it will automatically be enrolled
in any ongoing transactions.  Calling begin/commit/rollback or setAutoCommit
on the datasource or connection will not be allowed.  If you need to perform
these functions yourself, set `JtaManaged` to `false`

In terms of JPA persistence.xml:

- `JtaManaged=true` can be used as a 'jta-data-source'
- `JtaManaged=false` can be used as a 'non-jta-data-source'


<a name="maxOpenPreparedStatements"></a>
## maxOpenPreparedStatements

The maximum number of open statements that can be allocated
from the statement pool at the same time, or zero for no
limit.

NOTE - Some drivers have limits on the number of open
statements, so make sure there are some resources left
for the other (non-prepared) statements.


<a name="poolPreparedStatements"></a>
## poolPreparedStatements

If true, a statement pool is created for each Connection and
PreparedStatements created by one of the following methods are
pooled:

    public PreparedStatement prepareStatement(String sql);
    public PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency)


<a name="testOnBorrow"></a>
## testOnBorrow

If true connections will be validated before being returned
from the pool. If the validation fails, the connection is
destroyed, and a new conection will be retrieved from the
pool (and validated).

NOTE - for a true value to have any effect, the
ValidationQuery parameter must be set.


<a name="testOnReturn"></a>
## testOnReturn

If true connections will be validated before being returned
to the pool.  If the validation fails, the connection is
destroyed instead of being returned to the pool.

NOTE - for a true value to have any effect, the
ValidationQuery parameter must be set.


<a name="testWhileIdle"></a>
## testWhileIdle

If true connections will be validated by the idle connection
evictor (if any). If the validation fails, the connection is
destroyed and removed from the pool

NOTE - for a true value to have any effect, the
timeBetweenEvictionRunsMillis property must be a positive
number and the ValidationQuery parameter must be set.

# XADataSource

There are several ways to configure a XADataSource. Depending the underlying datasource (Oracle, MySQL one or the other
solution can be more adapted.

This part deals with `JtaManaged` XaDataSource since a not managed XaDataSource can be defined as a standard
resource using `class-name`.

## Single definition

First solution is to define as `JdbcDriver` an XADataSource:

    <Resource id="myXaDs" type="DataSource">
        JdbcDriver = org.foo.MyXaDataSource

        myXaProperty = value

        myPoolProperty = 10
    </Resource>

This solution merges properties for the XaDataSource and the pool (tomcat-jdbc for TomEE, dbcp for OpenEJB by default
but still configurable with DataSourceCreator).

Note: in this case for Oracle for instance you'll define UserName for the pool and User for the datasource which
can look weird if you don't know properties are used for 2 instances (pool and datasource).

Note: this solution uses the same logic than @DataSourceDefinition factory mecanism.

## Two resources definition

An alternative is to define a resource for the XaDataSource:

    <Resource id="myXa" class-name="org.foo.MyXaDataSource">
        myXaProperty = value
    </Resource>

And then wrap it in the pool:

    <Resource id="myXaDs" type="DataSource">
        DataSourceCreator = [dbcp|dbcp-alternative]
        myPoolProperty = 10
    </Resource>

Note: `dbcp` is more adapted than `dbcp-alternative` in most of the case because it is reusing direct dbcp JTA management.

## Known issues

For TomEE 1.7.0/1.7.1 you can need to add the property:

     openejb.datasource.pool = true

in resource properties to ensure the resource is pooled.


## Details about DataSource and their factories (advanced configuration)


[Configuration by creator](datasource-configuration-by-creator.html)
