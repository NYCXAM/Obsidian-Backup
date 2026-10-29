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

**Reliable flooding:** A router stores the most recent LSP from each source. When it receives a newer LSP, it adds it to its database, forwards it to all neighbors except the one that sent it, and recalculates routes. Duplicate or older LSPs are ignored. Routers generate LSPs periodically with increasing sequence numbers, decrement TTL while forwarding and aging stored LSPs; an LSP is discarded when its TTL reaches 0.

### Shortest-path routing
After flooding, each router uses **Dijkstra's algorithm** to find the lowest-cost paths from itself to every destination. Link costs must be non-negative.

- **Confirmed:** Nodes whose least-cost path is final.
- **Tentative:** Candidate paths that may still improve.

Start by confirming the local router with cost 0. Repeatedly confirm the tentative node with the smallest cost, then relax its links: for every unconfirmed neighbor, replace its tentative cost if the newly confirmed node gives a cheaper path. Each table entry records `<Destination, Cost, NextHop>`.

### OSPF and LS versus DV
**OSPF (Open Shortest Path First):** A link-state routing protocol that floods link-state advertisements and computes routes with Dijkstra's algorithm. OSPF runs directly over IP using protocol number 89, not TCP or UDP.

| | Link state | Distance vector |
| --- | --- | --- |
| Information shared | Direct-neighbor links and costs | Estimated cost to every destination |
| Scope of updates | Flooded throughout the routing domain | Sent only to direct neighbors |
| Path computation | Each router runs Dijkstra on a topology map | Routers iteratively apply Bellman-Ford |
| Common issue | More control traffic and local computation | Routing loops and count to infinity |
| Effect of a bad router | Can advertise incorrect local link costs | Can advertise incorrect path costs that other routers reuse |

**RFC:** A Request for Comments is an Internet standards publication, primarily produced through the IETF.

## Internet Protocol (IPv4)

**IP:** The network-layer protocol that lets heterogeneous networks operate as one internetwork. IP defines global addressing, datagram format, and packet-handling conventions. Routing protocols choose paths; IP forwards packets along those paths; ICMP reports network errors and signaling information.

### IP service model
IP provides a connectionless, best-effort datagram service. It does not guarantee delivery, order, uniqueness, or a bounded delay. TCP or an application protocol supplies reliability when needed.

### IPv4 datagram
An IPv4 datagram has a variable-length header, normally 20 bytes without options, followed by data such as a TCP segment or UDP datagram.

- **Version** and **header length:** Identify IPv4 and the header size.
- **Total length:** Size of the entire datagram in bytes.
- **Type of service:** Indicates the desired treatment of traffic, such as real-time versus non-real-time data.
- **Identifier, flags, fragment offset:** Support fragmentation and reassembly.
- **TTL:** Maximum remaining router hops. Each router decrements it, preventing packets from looping forever.
- **Protocol:** Identifies the upper-layer payload, such as TCP or UDP.
- **Header checksum:** Detects errors in the IP header only.
- **Source and destination addresses:** 32-bit IPv4 addresses.
- **Options:** Optional features such as timestamping, route recording, or minimum-MTU discovery.

### Fragmentation and reassembly
**MTU (Maximum Transmission Unit):** The largest network-layer packet a link can carry. If a router must send a datagram over a link with a smaller MTU, it may split the datagram into fragments. The destination reassembles fragments, keeping fragmentation transparent to higher layers.

Fragments carry the same identifier so the destination can group them. The **more fragments (MF)** flag is 1 on every fragment except the last. The **fragment offset** gives the fragment's data position relative to the original payload in units of 8 bytes. Therefore, every non-final fragment's data length must be a multiple of 8 bytes.

An unfragmented datagram has `MF = 0` and fragment offset 0. If the destination cannot collect all fragments before its reassembly timer expires, it discards the incomplete datagram.

Example: An original 1420-byte datagram with a 20-byte header contains 1400 bytes of data. With MTU 532, each full fragment carries 512 bytes of data:

| Fragment | Data length | Total length | Offset | MF |
| --- | ---: | ---: | ---: | ---: |
| 1 | 512 | 532 | 0 | 1 |
| 2 | 512 | 532 | 64 | 1 |
| 3 | 376 | 396 | 128 | 0 |

### Hierarchical IPv4 addressing
An IPv4 address is a globally unique 32-bit identifier for a network interface, written in dotted-quad notation such as `12.34.158.5`.

Addresses have a **network prefix** and a **host portion**. Routers forward based on network prefixes rather than individual hosts, which keeps forwarding tables manageable and lets a network add hosts without changing Internet-wide routes.

**Classful addressing:** The older system used fixed-size Class A (`/8`), B (`/16`), and C (`/24`) blocks. It wasted addresses and increased routing-table size, motivating subnetting and CIDR.

**Address allocation hierarchy:** ICANN allocates large address blocks to Regional Internet Registries, which allocate to ISPs and large institutions. ISPs can allocate smaller prefixes to customers, preserving hierarchical aggregation.

### Subnetting
Subnetting divides one assigned network into smaller internal networks by borrowing bits from the host portion. A **subnet mask** identifies the subnet bits. The network number is found with bitwise AND:

$$\text{SubnetNum} = \text{SubnetMask} \mathbin{\&} \text{DestinationAddress}$$

A router matches the destination against each `<SubnetNum, SubnetMask, NextHop>` entry. If the matching next hop is an interface, it delivers the datagram directly; otherwise, it forwards it to the next router. A default router handles destinations with no matching entry.

Subnets are usually visible only inside an organization, so its internal structure can remain hidden from the Internet.

### CIDR and prefix forwarding
**CIDR (Classless Inter-Domain Routing):** Uses a prefix of arbitrary length, written `a.b.c.d/x`, where `x` is the number of prefix bits. For example, `200.23.16.0/23` covers `200.23.16.0` through `200.23.17.255`.

CIDR enables **route aggregation**: contiguous networks can share one shorter prefix advertisement, reducing forwarding-table entries. If several prefixes match a destination, a router uses **longest-prefix matching**, selecting the most specific matching prefix. For example, `201.10.6.17` matches both `201.10.0.0/21` and `201.10.6.0/23`, so the router selects `/23`.

### DHCP
**Dynamic Host Configuration Protocol (DHCP):** Lets a host obtain network configuration automatically when it joins a network. The server leases addresses from a pool, allowing addresses to be reused and renewed.

1. **DHCPDISCOVER:** A host broadcasts to find a server, often using `255.255.255.255` before it has an address.
2. **DHCPOFFER:** A server offers an available address.
3. **DHCPREQUEST:** The host requests the offered address.
4. **DHCPACK:** The server confirms the lease.

DHCP can also provide the network mask, default gateway (first-hop router), and DNS server. Because routers normally do not forward local broadcasts, each subnet needs access to a DHCP server or **DHCP relay agent**. A relay forwards the client's broadcast as a unicast request to a server on another network, then forwards the server's response back to the client.

### Private addresses and NAT
**Private IPv4 addresses:** Addresses reserved for internal networks. Different organizations can reuse them, so they are not globally unique or routable on the public Internet.

| Private block | Address range |
| --- | --- |
| `10.0.0.0/8` | `10.0.0.0` through `10.255.255.255` |
| `172.16.0.0/12` | `172.16.0.0` through `172.31.255.255` |
| `192.168.0.0/16` | `192.168.0.0` through `192.168.255.255` |

**NAT (Network Address Translation):** Changes IP addresses as packets cross a network boundary. The address-and-port translation shown in the slides lets many private hosts share one public IPv4 address, using different ports to distinguish their traffic.

Example: A host at `10.0.0.1:3345` sends to a web server at `128.119.40.186:80`:

1. The NAT router replaces the outgoing **source** with its public endpoint, `138.76.29.7:5001`.
2. It records a mapping between `10.0.0.1:3345` and `138.76.29.7:5001` in its translation table.
3. When a reply arrives at `138.76.29.7:5001`, it replaces the **destination** with `10.0.0.1:3345` and forwards it internally.

NAT conserves public addresses and allows internal addressing to remain unchanged when the ISP changes. Ports are 16 bits, providing many possible mappings, but practical capacity also depends on the router and its mapping rules.

**Tradeoff:** NAT changes packets and requires state inside the network, weakening the end-to-end model. An outside host normally cannot initiate contact with a private host without a configured mapping or NAT traversal, complicating servers and peer-to-peer applications. NAT alone does not guarantee security.

### MAC addresses and ARP
**IP address:** A hierarchical network-layer address used to route a packet across networks. Its prefix depends on the network the interface joins.

**MAC address:** A link-layer address used to deliver a frame on the current LAN. Ethernet MAC addresses are normally 48 bits, written as six hexadecimal bytes, such as `1A-2F-BB-76-09-AD`. They are flat addresses, typically assigned by a manufacturer from allocated address blocks, and can also be changed in software.

**ARP (Address Resolution Protocol):** Finds the MAC address corresponding to an IPv4 address on the local link.

1. A checks its **ARP cache** for the target IP address.
2. If no entry exists, A broadcasts an ARP request to `FF:FF:FF:FF:FF:FF`, asking which interface owns that IP. The request includes A's IP and MAC addresses.
3. The owner replies directly to A with its MAC address. A caches the mapping and can send frames to it.

Cache entries are **soft state**: they expire unless refreshed. The cache stores `<IP address, MAC address, expiration>` and ARP learns mappings automatically.

**Local versus remote destination:** For a host on the same subnet, resolve that host's MAC address. For a remote destination, resolve the **next-hop router's MAC address**. The frame goes to the router, while the IP packet still names the final destination. Each router constructs a new link-layer frame for its next hop.

An ARP message contains the hardware/protocol types and address lengths, a request/reply operation, and the sender's and target's MAC/IP addresses.

### ICMP, ping, and traceroute
**ICMP (Internet Control Message Protocol):** Carries network-layer error reports and diagnostic/control messages inside IP packets. It informs the source about a problem rather than repairing it or guaranteeing delivery.

- **Destination unreachable:** A network, host, protocol, or port cannot be reached.
- **Time exceeded:** A packet's TTL reaches zero, or fragment reassembly takes too long.
- **Redirect:** A router informs a host that a better first-hop router is available.
- **Echo request/reply:** Diagnostic messages used by `ping`.

The **type** identifies the message category and the **code** gives a more specific reason. In ICMPv4, echo request is type 8, echo reply is type 0, destination unreachable is type 3, and time exceeded is type 11.

**Ping:** Sends echo requests and measures reply round-trip time and loss. No reply does not necessarily mean the host is down, since a network may filter ICMP.

**Traceroute:** Sends probes with increasing TTL values: 1, 2, 3, and so on. Each router where a probe's TTL expires returns an ICMP time-exceeded message, revealing successive hops and their round-trip times. The final response depends on the probe type, such as an echo reply or a UDP port-unreachable message.

### Tunneling and VPNs
**Tunnel:** Carries an entire original packet inside another packet, creating a logical link across an intermediate network.

At gateway A, the original packet becomes the payload of an **outer IP packet** addressed from gateway A to gateway B. Intermediate routers forward using the outer header. Gateway B removes that header and forwards the original packet using its unchanged **inner** addresses.

**VPN (Virtual Private Network):** Connects private hosts or networks across shared infrastructure. A secure VPN encrypts and authenticates tunneled traffic between its endpoints, allowing private communication across an untrusted network. Encapsulation by itself does not provide encryption.

Tunnels also carry protocols or multicast traffic through networks that do not directly support them. Costs include extra headers, processing, configuration, and a reduced effective MTU, which can cause fragmentation or require smaller packets.

## IPv6

**IPv6:** Uses 128-bit addresses rather than IPv4's 32 bits, providing a much larger address space. Addresses use colon-separated hexadecimal groups, such as `2001:db8::1`. `::` abbreviates one consecutive run of zero groups.

### IPv6 packet header
IPv6 has a **fixed 40-byte base header**. Optional information goes in extension headers rather than making the base header variable-length.

- **Version:** Identifies IPv6.
- **Traffic class and flow label:** Support traffic classification and identification of packets belonging to a flow.
- **Payload length:** Number of bytes after the base header, including extension headers.
- **Next header:** Identifies the following extension header or upper-layer protocol, such as TCP or UDP.
- **Hop limit:** The equivalent of IPv4 TTL, decremented at each router.
- **Source and destination:** 128-bit addresses.

**Differences from IPv4:** No base-header checksum, reducing per-hop checksum work. Error detection is handled at other layers. The base header also has no fragmentation fields: IPv6 routers do not fragment packets. If needed, the source fragments using a Fragment extension header, and the destination reassembles them.

### Address delivery types

| Type | Delivery |
| --- | --- |
| **Unicast** | One specific interface |
| **Multicast** | Interfaces belonging to a selected group |
| **Anycast** | One interface from a group sharing an address, usually the nearest according to routing cost |
| **Broadcast** | All interfaces on a local network in IPv4. IPv6 uses multicast instead of broadcast. |

### IPv4 and IPv6 coexistence
IPv4 and IPv6 headers are incompatible, so an IPv6 packet cannot simply pass through an IPv4-only router as a native packet.

- **Translation:** Converts between IPv4 and IPv6 packets. Some IPv6 information, such as its flow label, has no equivalent IPv4 field and may be lost.
- **Tunneling:** Wraps the complete IPv6 packet inside an IPv4 packet between tunnel endpoints. IPv4 routers use the outer IPv4 header. The exit removes it, preserving the original IPv6 header and payload.

## Transport Layer

*Source: lecture 8, `8_VPN_IPV6_TransportLayer.pptx`. VPN and IPv6 are covered above.*

### Process-to-process delivery and demultiplexing

The network layer delivers packets between hosts/interfaces. The **transport layer** provides logical communication between application processes, using **sockets** as application endpoints. Ports identify the receiving service; checksums detect corruption. Reliable delivery and flow control depend on the chosen transport protocol.

- **Multiplexing:** Collect outgoing data from multiple sockets and add headers so the flows can share the network.
- **Demultiplexing:** Use the received headers to deliver data to the correct socket.
- **UDP:** An ordinary unconnected receiving socket is selected by the destination IP and destination port. Datagrams from different senders can reach the same socket; the source IP/port provide a return address.
- **TCP:** An established connection is distinguished by `(source IP, source port, destination IP, destination port)`. Including the protocol gives the 5-tuple used below. Many clients can connect to the same server port because their connection tuples differ.

Applications normally use sockets in user space; the OS implements TCP/UDP and IP in the kernel, and the NIC/driver handles link transmission and receive/transmit buffers. Routers normally forward at IP rather than running the endpoints' TCP connection logic.

### UDP service, header, and checksum

**UDP:** A connectionless, message-oriented transport. Each datagram is independent. UDP adds process demultiplexing and error detection to IP, but does not itself provide retransmission, ordering, duplicate suppression, flow control, or congestion control.

- **8-byte header:** Source port, destination port, length, checksum; each field is 16 bits. Length includes the UDP header and payload.
- **Checksum:** Form a one's-complement sum of 16-bit words, folding carries back into the sum, then complement it. Coverage includes the IP pseudoheader, UDP header, and data. The receiver checks the sum including the checksum; all ones indicates no detected error, not proof that corruption is impossible.
- **Advantages:** No connection handshake, little per-connection protocol state, small header, and application control over when to send. UDP sockets still have OS buffers; “no connection state” does not mean “no buffering.”
- **Uses:** DNS/DHCP queries and delay-sensitive applications such as calls, games, and streaming. Applications can implement their own reliability when needed; UDP itself does not supply it.

### TCP byte stream, sequence numbers, and header

**TCP:** A connection-oriented, full-duplex, reliable, ordered **byte stream** over best-effort IP. IP may lose, corrupt, duplicate, delay, or reorder packets; TCP uses checksums, byte numbering, ACKs, buffering, and retransmission to hide these problems when communication succeeds. A persistent failure can still terminate the connection.

- TCP splits the stream into **segments**, not application messages. One `send()` need not correspond to one segment or one `recv()`.
- **MSS (maximum segment size):** Maximum TCP payload per segment, excluding TCP/IP headers. MTU limits the whole IP packet on a link. With MTU 1500 and ordinary 20-byte IPv4/TCP headers, the payload limit is 1460 bytes.
- **Sequence number:** The number of the first payload byte in a segment. The two directions have independent initial sequence numbers (ISNs).
- **ACK number:** The **next byte expected in order**. ACK $a$ cumulatively acknowledges all earlier bytes, not byte $a$ itself. It confirms TCP receipt, not that the application has read the data.
- **Example:** `SEQ=1001`, data length 100 carries bytes 1001–1100. If no earlier gap exists, the receiver replies `ACK=1101`.

**TCP header:** Minimum 20 bytes, plus options. Important fields are 16-bit source/destination ports, 32-bit sequence and ACK numbers, header length, flags, advertised receive window, checksum, urgent pointer, and options. Header length is expressed in 4-byte words. The checksum covers the TCP header/data and IP pseudoheader.

| Flag | Meaning |
| --- | --- |
| SYN | Synchronize sequence numbers when opening |
| ACK | ACK-number field is valid |
| FIN | Sender has no more stream data to send |
| RST | Abort/reset a connection |
| PSH | Request prompt delivery of buffered data; does not create message boundaries |
| URG | Urgent-pointer field is significant; identifies an urgent-data boundary |

### TCP establishment and termination

**Three-way handshake:** Exchange both ISNs and acknowledge each direction. SYN consumes one sequence number; an ACK without data does not.

| Step | A sends to B | B sends to A |
| --- | --- | --- |
| 1 | `SYN, SEQ=x` | |
| 2 | | `SYN+ACK, SEQ=y, ACK=x+1` |
| 3 | `ACK, SEQ=x+1, ACK=y+1` | |

The third packet can also carry application data. If the SYN or response is lost, timers cause retransmission; connection setup therefore adds latency and can be delayed by loss. The slide's example SYN timers are historical examples, not universal constants.

**Normal opening states:** Client: `CLOSED → SYN-SENT → ESTABLISHED`. Server: `LISTEN → SYN-RCVD → ESTABLISHED`.

**Orderly close:** Each direction closes independently with FIN/ACK. A typical exchange is `FIN → ACK → FIN → ACK`; ACK and FIN can be combined. FIN consumes one sequence number. Previously sent data is delivered before EOF; `recv()` returns 0 after the received stream has ended. `shutdown(..., SHUT_WR)` allows a sending-side close while continuing to receive.

- Active closer: `FIN-WAIT-1 → FIN-WAIT-2 → TIME-WAIT → CLOSED`.
- Passive closer: `CLOSE-WAIT → LAST-ACK → CLOSED`; the application must close its side.
- Simultaneous closes can enter `CLOSING`. **TIME-WAIT** allows the final ACK to be repeated and old segments to expire. **RST** aborts instead of completing an orderly drain.

## TCP Reliability and Sliding Windows

*Source: lecture 9, `9_TCP_flow_and_congestionControl.pptx`.*

### ARQ, loss, and duplicate suppression

**ARQ (Automatic Repeat reQuest):** Combine ACKs and retransmission timers to recover from loss. A generic protocol can use positive ACKs or negative ACKs (NACKs); TCP normally uses cumulative ACKs rather than explicit NACKs.

**Stop-and-wait:** Send one packet, retain a copy, and wait for its ACK before sending the next. On timeout, retransmit. A lost data packet, lost ACK, or premature timeout can cause retransmission. The receiver uses sequence numbers to recognize duplicates, ACKs them again, and avoids delivering the same bytes twice.

**32-bit sequence numbers wrap around:** Compare them using modulo arithmetic, not ordinary integer ordering. Within a valid comparison range, $a$ precedes $b$ when $0 < (b-a)\bmod 2^{32} < 2^{31}$. Old duplicates must not be confused with new data after wraparound. TCP timestamps support **PAWS** (Protection Against Wrapped Sequence Numbers).

### Stop-and-wait performance and pipelining

With packet size $L$ bits, link rate $R$ bits/s, and round-trip propagation delay RTT, ignoring ACK transmission time:

$$U_{stop\text{-}wait}=\frac{L/R}{RTT+L/R}$$

**Lecture example:** $R=1$ Gbps, one-way propagation 15 ms, $L=8000$ bits. Transmission takes 0.008 ms, but the send/ACK cycle takes about 30.008 ms. Throughput is only about **33.3 kB/s**, despite the fast link.

**Pipelining / sliding window:** Allow multiple unacknowledged packets in flight. Keep sent data buffered until acknowledged; advancing ACKs free space for new data. Sliding a fixed-size window changes its position, not necessarily its size. A one-packet window behaves like stop-and-wait.

- **Bandwidth-delay product:** $R\times RTT$ bits is approximately the in-flight data needed to keep the path busy, ignoring serialization/ACK overhead. Use consistent bit/byte units.
- With an allowed in-flight window of $W$ bytes, throughput is bounded roughly by $\min(R,8W/RTT)$ in bits/s. A large bandwidth-delay product requires a large window.

### Go-Back-N versus Selective Repeat

| | Go-Back-N | Selective Repeat |
| --- | --- | --- |
| ACKs | Cumulative; progress waits for the missing gap | Individually acknowledge received packets |
| Out-of-order data | Basic receiver discards it | Receiver buffers it |
| Timeout recovery | Retransmit the oldest missing packet and later outstanding packets | Retransmit only the missing/timed-out packet |
| State | Simpler; normally a timer for the oldest unacknowledged packet | More buffering and per-packet tracking/timers |

These are generic ARQ designs. TCP combines cumulative ACKs with selective retransmission behavior and can buffer out-of-order bytes; it is not identical to either simplified classroom protocol.

### Cumulative ACKs and gaps

The ACK stays at the first missing byte even if later data arrives. Once the gap is filled, it can jump across all consecutively buffered data. A later cumulative ACK can also cover data whose earlier ACK was lost.

**Lecture example:** Assume bytes before 234118 have already arrived and out-of-order data is buffered.

| Arrival: SEQ / data length | ACK sent | Reason |
| --- | --- | --- |
| 234118 / 100 | 234218 | Bytes through 234217 are in order |
| 234268 / 120 | 234218 | Missing bytes 234218–234267 |
| 234388 / 100 | 234218 | Same gap remains |
| Retransmitted 234218 / 50 | 234488 | Gap filled; buffered data now forms one continuous range |

Duplicate ACKs suggest a gap, but reordering can produce them too. A single duplicate ACK is not conclusive proof of loss.

### RTT estimation and retransmission timeout

**RTT:** Time from sending a segment to receiving its acknowledgment. Timeout too short causes unnecessary retransmissions; too long delays loss recovery. RTT changes with queueing, so use measured samples and a smoothed estimate.

The lecture's original estimator weights the **old estimate** by $\alpha$ (about 0.8–0.9):

$$EstimatedRTT_{new}=\alpha EstimatedRTT_{old}+(1-\alpha)SampleRTT$$
$$RTO_{lecture}=2\times EstimatedRTT$$

This is the slide's simplified/original timeout model, not a complete production TCP RTO formula. The **10-byte TCP timestamp option** contains kind/length, `TSval` (sender timestamp), and `TSecr` (echoed timestamp), supporting RTT measurement and PAWS.

**Karn–Partridge:** An ACK after retransmission is ambiguous: did it acknowledge the original send or the retransmission? Without disambiguating timestamps, do not use that retransmitted data for an RTT sample. Resume sampling unambiguously acknowledged transmissions. On repeated retransmission timeouts, **exponentially back off** the RTO, typically doubling it; a persist probe or every fast retransmission is not automatically an RTO-doubling event.

## TCP Flow Control and Transmission Efficiency

*Sources: lecture 9, slides 90–99; lecture 10, `10_TCP_flow_and_congestionControl_p2.pptx`, slides 2–39.*

### Receive window and effective sending window

**Flow control protects the receiver:** The application may consume bytes more slowly than TCP receives them. The receiver advertises its available receive-buffer capacity as **AdvertisedWindow / rwnd** in the TCP header.

$$rwnd=MaxRcvBuffer-\text{buffered, unread bytes}$$

In the lecture's simplified contiguous-data model:

$$rwnd=MaxRcvBuffer-((NextByteExpected-1)-LastByteRead)$$

Actual buffer accounting must also consider stored out-of-order data. The sender tracks application-written bytes, sent bytes, and acknowledged bytes; sent-but-unacknowledged bytes must remain available for retransmission.

$$BytesInFlight=LastByteSent-LastByteACKed$$
$$EffectiveWindow=\max(0,rwnd-BytesInFlight)$$

This flow-control-only expression measures room for **additional** data, not the total window. Example: `rwnd=8000`, with 3000 bytes outstanding, permits 5000 more bytes.

**Zero window:** Stop ordinary new-data transmission. A **persist timer** periodically triggers a small window probe so a lost window-update ACK cannot leave both sides waiting forever. A zero receive window is receiver backpressure, not proof of network congestion.

### Silly window syndrome, delayed ACKs, and Nagle

**Silly window syndrome:** Tiny writes or tiny increments in free receive-buffer space cause many small segments, wasting bandwidth on headers and processing. MSS is a maximum, not a requirement that every segment be full.

- **Receiver side:** Avoid advertising every tiny increment of newly available space. ACKs/window updates can be coalesced; delayed ACKs reduce ACK traffic but cannot wait indefinitely. The lecture cites the historical rule of delay below 500 ms and ACKing at least every second full-sized segment; 200/500 ms examples are not universal OS defaults.
- **Nagle's algorithm, sender side:** If a full MSS can be sent within the allowed window, send it. Otherwise, while data is unacknowledged, buffer small new writes. When outstanding data is acknowledged, a small segment may be sent without waiting to fill an MSS, subject to the window.
- **Interaction:** Nagle may wait for an ACK while the receiver delays that ACK awaiting another segment. The delayed-ACK timer eventually releases the stall, but interactive latency increases.
- **Socket options:** `TCP_NODELAY` disables Nagle. Linux `TCP_QUICKACK` requests quick ACK behavior; it is platform-specific and not a permanent, portable “disable delayed ACK” switch. [Linux TCP API](https://man7.org/linux/man-pages/man7/tcp.7.html)

TCP may send a partial segment because of its sending policy or a push request; **PSH is not the retransmission timer and does not guarantee one packet per application write**.

## TCP Congestion Control

*Source: lecture 10, slides 40–69.*

**Congestion control protects the network**, while flow control protects the receiving host. Multiple flows compete for link bandwidth and router queue space. A drop-tail FIFO queue serves packets in arrival order and drops new arrivals when full.

- **Congestion:** Offered load exceeds capacity, causing queueing delay and packet drops.
- **Goodput:** Useful application data delivered per second, excluding duplicate retransmissions and protocol overhead.
- **Congestion collapse:** More offered traffic produces less useful work. Loss causes retransmissions, which increase load and cause still more loss.
- **End-host signals:** Rising RTT suggests growing queues; retransmission timeouts and repeated duplicate ACKs indicate possible loss. The sender observes end-to-end symptoms rather than directly seeing router queues.

**Congestion window (cwnd):** Sender-maintained limit on outstanding data, separate from receiver-advertised `rwnd`:

$$SendWindow=\min(cwnd,rwnd)$$
$$AdditionalData\leq\max(0,\min(cwnd,rwnd)-BytesInFlight)$$

ACKs release outstanding data and help **self-clock** new transmissions. Increase cautiously when the path succeeds; reduce the sending rate when congestion is detected.

**AIMD (additive increase, multiplicative decrease):** In the lecture's congestion-avoidance model, increase `cwnd` roughly one MSS per successful window/RTT and halve it on a congestion-loss event. This creates a rising-and-falling sawtooth pattern. It is a simplified model: timeout recovery and other TCP phases/variants do not always just halve the window.

**Example:** With `rwnd=16 KB`, `cwnd=8 KB`, and 3 KB in flight, at most **5 KB** of additional data can be sent. A large receive window does not override the smaller congestion window.

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
