index-group=Unrevised
type=page
status=published
title=Common PersistenceProvider properties
~~~~~~
While not a definitive list, it does help to show a side-by-side view of
common properties used by the various persistence providers out there.

<a name="CommonPersistenceProviderproperties-TopLink"></a>
# TopLink


    <properties>
     
      <!--http://www.oracle.com/technology/products/ias/toplink/JPA/essentials/toplink-jpa-extensions.html-->
      <property name="toplink.ddl-generation" value="drop-and-create-tables"/>
      <property name="toplink.logging.level" value="FINEST"/>
      <property name="toplink.ddl-generation.output-mode" value="both"/>
      <property name="toplink.target-server" value="pl.zsk.samples.ejbservice.OpenEJBServerPlatform"/>
    </properties>


<a name="CommonPersistenceProviderproperties-OpenJPA"></a>
# OpenJPA


    <properties>
      <!--http://openjpa.apache.org/faq.html-->
      <!-- does not create foreign keys, creates schema and deletes content of a database
           (deleteTableContents - foreign keys are created twice???), use dropDB instead -->
      <property name="openjpa.jdbc.SynchronizeMappings" value="buildSchema(foreignKeys=true,schemaAction='dropDB,add')"/>
      <!--Resolves the problem with foreign key integrity - joined entities are persisted sometimes in wrong order??? (verify it)-->
      <property name="openjpa.jdbc.SchemaFactory" value="native(foreignKeys=true)" />
      <!--Create foreign keys-->
      <property name="openjpa.jdbc.MappingDefaults" value="ForeignKeyDeleteAction=restrict, JoinForeignKeyDeleteAction=restrict"/>
      <property name="openjpa.Log" value="DefaultLevel=TRACE,SQL=TRACE" />
    </properties>


<a name="CommonPersistenceProviderproperties-Hibernate"></a>
# Hibernate


    <properties>
      <property name="hibernate.hbm2ddl.auto" value="create-drop"/>
      <property name="hibernate.transaction.manager_lookup_class" value="org.apache.openejb.hibernate.TransactionManagerLookup"/>
    </properties>
