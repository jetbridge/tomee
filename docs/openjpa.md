index-group=JPA
type=page
status=published
title=OpenJPA
~~~~~~
OpenJPA is bundled with OpenEJB as the default persistence provider.

An example of working `persistence.xml` for OpenJPA:

    <persistence xmlns="http://java.sun.com/xml/ns/persistence" version="1.0">

      <persistence-unit name="movie-unit">
        <jta-data-source>movieDatabase</jta-data-source>
        <non-jta-data-source>movieDatabaseUnmanaged</non-jta-data-source>
        <class>org.superbiz.injection.jpa.Movie</class>

        <properties>
          <property name="openjpa.jdbc.SynchronizeMappings" value="buildSchema(ForeignKeys=true)"/>
        </properties>
      </persistence-unit>
    </persistence>

    
Where the datasources above are configured in your openejb.xml as follows:
    
    <Resource id="movieDatabase" type="DataSource">
      JdbcDriver = org.hsqldb.jdbcDriver
      JdbcUrl = jdbc:hsqldb:mem:moviedb
    </Resource>
    
    <Resource id="movieDatabaseUnmanaged" type="DataSource">
      JdbcDriver = org.hsqldb.jdbcDriver
      JdbcUrl = jdbc:hsqldb:mem:moviedb
      JtaManaged = false
    </Resource>


Or in properties as follows:

    p.put("movieDatabase", "new://Resource?type=DataSource");
    p.put("movieDatabase.JdbcDriver", "org.hsqldb.jdbcDriver");
    p.put("movieDatabase.JdbcUrl", "jdbc:hsqldb:mem:moviedb");

    p.put("movieDatabaseUnmanaged", "new://Resource?type=DataSource");
    p.put("movieDatabaseUnmanaged.JdbcDriver", "org.hsqldb.jdbcDriver");
    p.put("movieDatabaseUnmanaged.JdbcUrl", "jdbc:hsqldb:mem:moviedb");
    p.put("movieDatabaseUnmanaged.JtaManaged", "false");

    
# Common exceptions
    
## Table not found in statement
    
Or as derby will report it "Table 'abc.xyz' doesn't exist"
    
Someone has to create the database structure for your persistent objects.
If you add the *openjpa.jdbc.SynchronizeMappings* property as shown below
OpenJPA will auto-create all your tables, all your primary keys and all
foreign keys exactly to match your objects.
    
    <persistence xmlns="http://java.sun.com/xml/ns/persistence" version="1.0">
    
     <persistence-unit name="movie-unit">
       <!-- your other stuff -->
    
       <properties>
         <property name="openjpa.jdbc.SynchronizeMappings" value="buildSchema(ForeignKeys=true)"/>
       </properties>
     </persistence-unit>
    </persistence>


<a name="OpenJPA-Auto-commitcannotbesetwhileenrolledinatransaction"></a>
## Auto-commit can not be set while enrolled in a transaction

Pending

<a name="OpenJPA-Thisbrokerisnotconfiguredtousemanagedtransactions."></a>
## This broker is not configured to use managed transactions.

<a name="OpenJPA-Setup"></a>
### Setup

Using a EXTENDED persistence context ...

    @PersistenceContext(unitName = "movie-unit", type = PersistenceContextType.EXTENDED)
    EntityManager entityManager;

on a persistence unit setup as RESOURCE_LOCAL ...

    <persistence-unit name="movie-unit" transaction-type="RESOURCE_LOCAL">
      ...
    </persistence-unit>

    
inside of a transaction.

    @TransactionAttribute(TransactionAttributeType.REQUIRED)
    public void addMovie(Movie movie) throws Exception {
        entityManager.persist(movie);
    }


Note the default transaction attribute for any ejb method is REQUIRED
unless explicitly set otherwise in the bean class or method in question.

<a name="OpenJPA-Solutions"></a>
### Solutions

You can either:

1. Change your bean or it's method to use `TransactionAttributeType.NOT_REQUIRED` or `TransactionAttributeType.NEVER` which will guarantee it will not be invoked in a JTA transaction.
1. Change your persistence.xml so the persistence-unit uses `transaction-type="TRANSACTION"` making it capable of participating in a JTA transaction.
