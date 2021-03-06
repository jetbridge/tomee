index-group=Configuration
type=page
status=published
title=EJB over SSL
~~~~~~

It is possible to setup client/server requests over SSL.  EJB requests from a remote client can happen two different ways:

 - **https** for when an EJB is running in TomEE
 - **ejbds** for when an EJB is running in OpenEJB Standalone

Note, TomEE can be setup to support **ejbds**.

# https

First, you'll need to setup Tomcat (TomEE) with SSL as described here:

  [http://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html](http://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html)

Once that is done and the `tomee` webapp can be accessed with `https`, an EJB client can invoke over `https` using the following
`InitialContext` setup:


    Properties p = new Properties();
    p.put("java.naming.factory.initial", "org.apache.openejb.client.RemoteInitialContextFactory");
    p.put("java.naming.provider.url", "https://127.0.0.1:8443/tomee/ejb");
    // user and pass optional
    p.put("java.naming.security.principal", "myuser");
    p.put("java.naming.security.credentials", "mypass");

    InitialContext ctx = new InitialContext(p);

    MyBean myBean = (MyBean) ctx.lookup("MyBeanRemote");

If you setup Tomcat (TomEE) to use the APR (Apache Portable Runitme) implementation of SSL on the server side, and you have connection issues like connection reset, you'll have to set 'https.protocols' system property.
'https.protocols' property must be set according to the SSLProtocol parameter of the HTTPS connector configuration :

[http://tomcat.apache.org/tomcat-7.0-doc/config/http.html][1]

You can also have a look a this : 

[http://docs.oracle.com/javase/1.4.2/docs/guide/plugin/developer_guide/faq/troubleshooting.html][2]

# ejbds

The SSL version of the `ejbd` protocol is called `ejbds` and is enabled and setup in OpenEJB Standalone by default.

Its configuration `conf/ejbds.properties` looks like this:

    server      = org.apache.openejb.server.ejbd.EjbServer
    bind        = 127.0.0.1
    port        = 4203
    disabled    = false
    threads     = 200
    backlog     = 200
    secure      = true
    discovery   = ejb:ejbds://{bind}:{port}

To access this service from a remote client, the `InitialContext` would be setup like the following:

    Properties p = new Properties();
    p.put("java.naming.factory.initial", "org.apache.openejb.client.RemoteInitialContextFactory");
    p.put("java.naming.provider.url", "ejbd://localhost:4201");
    // user and pass optional
    p.put("java.naming.security.principal", "myuser");
    p.put("java.naming.security.credentials", "mypass");

    InitialContext ctx = new InitialContext(p);

    MyBean myBean = (MyBean) ctx.lookup("MyBeanRemote");

## Changing the Cipher Suite
[This is a pending feature](https://issues.apache.org/jira/browse/OPENEJB-1856)
By default, the ejbds protocol connects with SSL_DH_anon_WITH_RC4_128_MD5. That means your connection is encrypted and the integrity of the transmission is verified. However, this only protects your from eavesdroppers, it offers absolutely zero protection from Man in the Middle attacks. This sort of attack could be pulled off without your knowledge and the attacker has the ability to intercept, monitor, and even modify your messages. If the attacker could control a router on your connection path, this attack could be trivially pulled off with nothing more but the OpenEJB server and client.

To secure your connections against this sort of attack, your client can cryptographically prove it's talking to the correct server before sending any data. To do this, simply select one or more secure cipher suites that your J2SE provider supports from [this listing](http://docs.oracle.com/cd/E19728-01/820-2550/cipher_suites.html).

You must now instruct the client and server to use that suite.

On the server:

    server      = org.apache.openejb.server.ejbd.EjbServer
    bind        = 127.0.0.1
    port        = 4203
    disabled    = false
    threads     = 200
    backlog     = 200
    secure      = true
    enabledCipherSuites = TLS_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA
    discovery   = ejb:ejbds://{bind}:{port}

On the client, you must supply a property:

    -Dopenejb.client.enabledCipherSuites=TLS_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA

The final piece is to make sure your server has available a private certificate that the the client can trust. This can be certificate from an authority or a self signed certificate. The javax.net.ssl.trustStore and javax.net.ssl.keyStore JVM properties [are used to set this up.](http://fusesource.com/docs/broker/5.3/security/SSL-SysProps.html)


  [1]: http://tomcat.apache.org/tomcat-7.0-doc/config/http.html
  [2]: http://docs.oracle.com/javase/1.4.2/docs/guide/plugin/developer_guide/faq/troubleshooting.html
