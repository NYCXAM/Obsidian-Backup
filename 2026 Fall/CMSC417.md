## Foundations and Internetworking

### Internet basics
**Internet:** A network of networks: end systems connect through access Internet Service Provider (ISPs), which interconnect through regional networks, large tier-1 ISPs, and Internet exchange points (IXPs).

- **Hosts / end systems:** Devices running network applications, such as clients and servers.
- **Communication links:** Fiber, copper, radio, or satellite; their transmission rate is the **bandwidth**.
- **Packet switches:** Routers and switches that forward chunks of data called packets.
- **Protocol:** Defines message formats, message order, and the actions taken when messages are sent or received. Internet standards are commonly published as RFCs by the IETF.
- The Internet also provides applications with a programming interface for sending and receiving data.

### Circuit switching vs. packet switching
**Circuit switching:** Reserves end-to-end resources for a call. It provides predictable performance, but reserved capacity sits idle when the sender has no data. (E.g. If your phone call reserves 1 Mbps but you are silent, that 1 Mbps remains reserved for you and cannot be temporarily used by someone else.)

**Packet switching:** Splits data into packets that share links. Routers queue packets when the output link is busy; a full buffer drops arriving packets.

**Multiplexing:** Multiple logical flows share one physical link.

### Switches vs Routers
**Switches:** A switch connects devices inside the same local network, such as computers in one home, office, or classroom.
- Devices connect to the switch.
- The switch forwards Ethernet **frames** based mainly on **MAC addresses**.
- It provides local connectivity, but it generally does not choose a path between separate networks.
- The several switches shown are still part of one larger connected local network.

**Routers:** A router connects separate networks together.
- Each cloud represents a different network or subnet.
- Routers sit between those networks.
- They forward IP **packets** toward their destination using IP addresses and routing tables.
- A packet may pass through several routers before reaching the destination network.
### Network edge and core
- **Network edge:** Hosts, access networks, and physical media. Common access networks include home Ethernet/Wi-Fi, enterprise Ethernet, and cellular networks.
- **Network core:** A mesh of routers that forwards packets hop by hop.
- **Forwarding:** Move an arriving packet to the correct output link using a local forwarding table.
- **Routing:** Determine the end-to-end path that packets take through routers.

### Delay, loss, and throughput
**Nodal delay:**
$$d_{total} = d_{proc} + d_{queue} + d_{trans} + d_{prop}$$

- $d_{proc}$: Processing delay for checks such as bit-error detection and choosing an output link.
- $d_{queue}$: Time waiting in the output buffer; increases with congestion.
- $d_{trans} = L/R$: Time to put an $L$-bit packet onto a link with rate $R$ bps.
- $d_{prop} = d/v$: Time for a signal to travel distance $d$ at propagation speed $v$.
- **Packet loss:** Occurs when a packet reaches a full, finite buffer. Recovery may involve retransmission, depending on the protocol.
- **Throughput:** Rate at which bits transfer between sender and receiver. End-to-end throughput is constrained by the bottleneck link; with shared links, connections split available capacity.

### Layering
Layering organizes a complex network into modules. Each layer provides a service to the layer above and relies on the service below, so a layer's implementation can change without changing the others.

**Internet protocol stack:**
- **Application:** Network applications such as HTTP, SMTP, and FTP.
- **Transport:** Process-to-process delivery, e.g., TCP and UDP.
- **Network:** Source-to-destination packet routing, e.g., IP and routing protocols.
- **Link:** Transfer between neighboring network elements, e.g., Ethernet and Wi-Fi.
- **Physical:** Transmit raw bits over the medium.

**Encapsulation:** At the sender, each layer adds its own header to the data from the layer above. At the receiver, the corresponding layer removes that header. Common units are a message, segment, packet/datagram, and frame.

### Internetworking and routing
Connecting hosts requires:
1. **Addressing:** A global identifier and a link-level name for neighboring nodes.
2. **Forwarding:** Switching packets between links.
3. **Routing:** Determining paths between hosts.

**Routing graph:** Model routers as nodes $N$ and links as edges $E$. Each link has a cost; a path's cost is the sum of its link costs. Costs may reflect hop count, bandwidth, or congestion.

**Why dynamic routing?** Static routes cannot respond to link/node failures, new links or nodes, or changing edge costs.

**Distance-vector routing:**
- Each router maintains a vector of its current costs to every destination and initially knows the cost to its direct neighbors.
- Routers advertise their distance vector to immediate neighbors in routing packets.
- A router saves the latest vector from each neighbor and recalculates when a received vector changes or a neighbor link fails/changes cost.
- Distance vector and link state are the two main classes of dynamic routing protocols.

## Routing Algorithms

### Distance-vector routing in detail
Each router's routing table stores a destination, its current least-cost estimate, and the next hop. Initially, it knows only itself at cost 0 and its directly connected neighbors; all other destinations have cost $\infty$.

**Bellman-Ford equation:**
$$d_x(y) = \min_{v \in N(x)} \{c(x,v) + d_v(y)\}$$

- $d_x(y)$: Least cost from router $x$ to destination $y$.
- $c(x,v)$: Cost of the direct link from $x$ to neighbor $v$.
- $d_v(y)$: Neighbor $v$'s advertised cost to $y$.
- The neighbor giving the minimum becomes the next hop in $x$'s forwarding table.

**How updates work:**
1. Each router sends its distance vector to direct neighbors.
2. It saves the latest vector received from each neighbor.
3. For each destination, it recomputes the best cost with the Bellman-Ford equation.
4. If any cost changes, it advertises its updated vector to neighbors.

Routers send advertisements periodically, typically every 30 seconds in RIP, and through **triggered updates** after a routing-table change, link failure, or link-cost change. The distributed process converges when no router can improve any estimate.
```
for all destinations y in N:
	D_x(y)=c(x, y)   //if y is not a neighbor then c(x, y) = inf
for each neeighbor w:
	D_w(y) = ? for all destination y in N.
for each neighbor w:
	send distance vector D_x = [D_x(y): y in N] to w	
	
while true:
	wait(until I receive any distance vector from some neighbor w)
	for each y in N:
		D_x(y) = minv{c(x, v) + D_v(y)}
		if D_x(y) is changed for any destination y:
			send distance vector D_x = [D_x(y):y in N] to all neighbors
```
### Link failures and count to infinity
After a link cost increases or a link fails, neighboring routers can incorrectly believe that the other still has a route to the destination. They then keep advertising increasingly expensive routes through each other. This slow, looping increase is the **count-to-infinity problem**.

**Mitigations:**
- **Small infinity:** Use a finite value to mean unreachable. RIP uses hop count, with 15 as the largest reachable distance and 16 as infinity.
- **Split horizon:** Do not advertise a route back to the neighbor from which that route was learned.
- **Poisoned reverse:** Advertise that route back to the learning neighbor with cost $\infty$, explicitly telling it not to use this router for that destination.

Poisoned reverse prevents many two-router loops, but it cannot detect every loop involving three or more routers. Routing tables also need to age out old information, so stale advertisements do not persist indefinitely.

**RIP:** Routing Information Protocol is a distance-vector protocol. Its small maximum hop count makes it suitable only for relatively small networks. Other distance-vector limitations include routing loops and static, predetermined link costs.

### Link-state routing
Distance vector learns routes through neighbors' estimates. **Link-state routing** gives every router a consistent view of the network topology.

- Each router creates a **link-state packet (LSP)** describing only its directly connected neighbors and their link costs.
- The router distributes the LSP to all routers, not only to its immediate neighbors.
- After collecting LSPs from every router, each router can construct the same network graph and calculate routes locally.

An LSP contains:
1. The ID of the router that created it.
2. The costs of links to its direct neighbors.
3. A sequence number, so routers can identify newer information.
4. A time-to-live (TTL), so stale information eventually disappears.

**RFC:** A Request for Comments is an Internet standards publication, primarily produced through the IETF.

## Socket Programming in C

**Socket:** An abstraction that lets an application send and receive data through a network, much like a file handle lets a program read and write a file. In C, a socket is represented by a file descriptor (`sockfd`).

### IP addresses and ports
- An IPv4 address has the form `x.x.x.x`, where each `x` is one byte from 0 to 255. It identifies a network interface, not necessarily an entire host.
- A host can have multiple network interfaces, such as Ethernet and Wi-Fi, and each interface can have its own IP address.
- A **port** is a 16-bit number from 0 to 65535 that identifies the application or service on that host.
- An endpoint is commonly written as `IP-address:port`, such as `10.10.10.10:5000`.

Think of the IP address as an apartment building and the port as an apartment number. TCP or UDP specifies how the data should be delivered.

### TCP and UDP sockets
**Stream socket:** Uses TCP over IP. TCP provides a reliable, ordered byte stream, but it does not preserve application message boundaries.

**Datagram socket:** Uses UDP over IP. UDP sends separate, best-effort datagrams. A datagram can be up to about 65,500 bytes, but it may be lost, duplicated, or arrive out of order.

**TCP connection identifier:**
$$\{\text{source IP},\ \text{source port},\ \text{protocol},\ \text{destination IP},\ \text{destination port}\}$$

This 5-tuple distinguishes one TCP connection from another. An application can use multiple sockets at once.

### TCP client-server flow
**Server:**
1. `socket()`: Create a TCP socket.
2. `bind()`: Assign a local IP address and port.
3. `listen()`: Mark the socket as ready to receive connection requests; `backlog` limits the waiting queue.
4. `accept()`: Block until a client connects, then return a new socket for communicating with that client. The original listening socket remains available for more clients.

**Client:**
1. `socket()`: Create a TCP socket.
2. `connect()`: Establish a connection to the server's address and port.

Both sides then communicate with `send()` and `recv()`, then call `close()` when finished. `shutdown()` can close only the sending side, receiving side, or both.

**Important:** `send()` may write fewer bytes than requested, and `recv()` may return fewer bytes than requested. Always check the return value and loop until the needed data has been handled.

### UDP changes
- Create the socket with `SOCK_DGRAM` and `IPPROTO_UDP`.
- UDP does not establish a connection, so it normally does not use `listen()` or `accept()`.
- Use `sendto()` to send a datagram to a specified address and `recvfrom()` to receive a datagram and its sender's address.
- A UDP socket can call `connect()`, but this only records a default peer locally; it does not create a TCP-style connection.

### Byte order
Different machines store multi-byte integers differently:
- **Little endian:** Least significant byte comes first.
- **Big endian:** Most significant byte comes first.
- **Network byte order:** Big endian.

Convert multi-byte numeric fields from host to network byte order before sending, then convert them back after receiving. Single-byte values do not need conversion.

### I/O multiplexing
**I/O multiplexing:** One event loop watches multiple file descriptors and reacts when one is ready for reading, writing, or accepting a connection.

- `select()` watches sets of descriptors for read, write, and exception events.
- `poll()` watches an array of descriptors and requested events.
- `epoll()` provides a more efficient Linux-specific interface for many descriptors.

Useful helpers: `inet_pton()` converts an IP address from text to binary, `inet_ntop()` converts it back to text, `getaddrinfo()` resolves a host/service, and `sockaddr_in` stores IPv4 address information.
