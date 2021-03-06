index-group=Discovery and Failover
type=page
status=published
title=Multipoint (TCP) Discovery
~~~~~~

As TCP has no real broadcast functionality to speak of, communication of
who is in the network is achieved by each server having a physical
connection to each other server in the network.

To join the network, the server must be configured to know the address of
at least one server in the network and connect to it.  When it does both
servers will exchange the full list of all the other servers each knows
about.	Each server will then connect to any new servers they've  just
learned about and repeat the processes with those new servers.	The end
result is that everyone has a direct connection to everyone 100% of the
time, hence the made-up term "multipoint" to describe this situation of
each server having multiple point-to-point connections which create a fully
connected graph.

On the client side things are similar.	It needs to know the address of at
least one server in the network and be able to connect to it.  When it does
it will get the full (and dynamically maintained) list of every server in
the network.  The client doesn't connect to each of those servers
immediately, but rather consults the list in the event of a failover, using
it to decide who to connect to next.

The entire process is essentially the art of using a statically maintained
list to bootstrap getting the more valuable dynamically maintained list.

# Server Configuration

In the server this list can be specified via the
`conf/multipoint.properties` file like so:

    server	    = org.apache.openejb.server.discovery.MultipointDiscoveryAgent
    bind	    = 127.0.0.1
    port	    = 4212
    disabled    = false
    initialServers = 192.168.1.20:4212, 192.168.1.30:4212, 192.168.1.40:4212


The above configuration shows the server has an `port` `4212` open for
connections by other servers for multipoint communication.  The
`initialServers` list should be a comma separated list of other similar
servers on the network.  Only one of the servers listed is required to be
running when this server starts up -- it is not required to list all
servers in the network.

# Client Configuration

Configuration in the client is similar, but note that EJB clients do not
participate directly in multipoint communication and do **not** connect to
the multipoint port.  The server list is simply a list of the regular
`ejbd://` urls that a client normally uses to connect to a server.

    Properties p = new Properties();
    p.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.openejb.client.RemoteInitialContextFactory");
    p.put(Context.PROVIDER_URL, "failover:ejbd://192.168.1.20:4201,ejbd://192.168.1.30:4201");
    InitialContext remoteContext = new InitialContext(p);

Failover can work entirely driven by the server, the client does not need
to be configured to participate.  A client can connect as usual to the server.

    Properties p = new Properties();
    p.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.openejb.client.RemoteInitialContextFactory");
    p.put(Context.PROVIDER_URL, "ejbd://192.168.1.20:4201");
    InitialContext remoteContext = new InitialContext(p);

If the server at `192.168.1.20:4201` supports failover, so will the client.

In this scenario the list of servers used for failover is supplied entirely
by the server at `192.168.1.20:4201`.  The server could have aquired the list
via multicast or multipoint (or both), but this detail is not visible to the
client.
