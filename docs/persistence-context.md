index-group=JPA
type=page
status=published
title=persistence-context
~~~~~~
<a name="persistence-context-Viaannotation"></a>
#  Via annotation

Both lookup and injection of an EntityManager can be configured via the
`@PersistenceContext` annotation.

    package org.superbiz;

    import javax.persistence.PersistenceContext;
    import javax.persistence.EntityManager;
    import javax.ejb.Stateless;
    import javax.naming.InitialContext;

    @Stateless
    @PersistenceContext(name = "myFooEntityManager", unitName = "foo-unit")
    public class MyBean implements MyInterface {

        @PersistenceContext(unitName = "bar-unit")
        private EntityManager myBarEntityManager;

        public void someBusinessMethod() throws Exception {
            if (myBarEntityManager == null) throw new NullPointerException("myBarEntityManager not injected");

            // Both can be looked up from JNDI as well
            InitialContext context = new InitialContext();
            EntityManager fooEntityManager = (EntityManager) context.lookup("java:comp/env/myFooEntityManager");
            EntityManager barEntityManager = (EntityManager) context.lookup("java:comp/env/org.superbiz.MyBean/myBarEntityManager");
        }
    }


<a name="persistence-context-Viaxml"></a>
# Via xml

The above @PersistenceContext annotation usage is 100% equivalent to the
following xml.

    <persistence-context-ref>
        <persistence-context-ref-name>myFooEntityManager</persistence-context-ref-name>
        <persistence-unit-name>foo-unit</persistence-unit-name>
        <persistence-context-type>Transaction</persistence-context-type>
    </persistence-context-ref>

    <persistence-context-ref>
        <persistence-context-ref-name>org.superbiz.calculator.MyBean/myBarEntityManager</persistence-context-ref-name>
        <persistence-unit-name>bar-unit</persistence-unit-name>
        <persistence-context-type>Transaction</persistence-context-type>
        <injection-target>
            <injection-target-class>org.superbiz.calculator.MyBean</injection-target-class>
            <injection-target-name>myBarEntityManager</injection-target-name>
        </injection-target>
    </persistence-context-ref>
