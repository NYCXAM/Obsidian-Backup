# CMSC417 Exam 1 Study Guide

## Scope and study priorities

**Scope:** Everything before NAT: lecture decks 1-6 and the DHCP portion of deck 7. The supplied Exam 1 review also states that NAT is not included. In deck 7, the DHCP/relay material is on slides 2-15; slide 16 starts private address space and NAT.

**Not part of the main guide:** NAT, ARP, detailed ICMP/ping/traceroute, VPNs, IPv6, BGP policy, Mobile IP, certificates, and advanced TCP features. Some appear in your full course note or old exams, but that does not put them in this exam's stated scope. Basic IP-versus-link addressing and the role of ICMP in the network layer still belong to the earlier lectures.

**Source priority:** Your stated cutoff and the supplied review determine scope. Lecture decks provide the content. [[CMSC417]] supplies explanations and terminology. The 2015, 2017, and 2021 exams supply practice styles, not predictions of the current exam's format, timing, or topic weights.

The review explicitly emphasizes:

- Internet components, switching, network structure, delay/loss/throughput, and layering.
- DV calculations, advertisements, Bellman-Ford, failures, count to infinity, split horizon, and poisoned reverse.
- Link-state strategy, reliable flooding, shortest-path calculation, and LS versus DV.
- IPv4 fragmentation/reassembly, classful addressing, subnetting, CIDR, longest-prefix matching, DHCP, and DHCP relay.

**Preparation order:** First practice calculations and algorithm traces: DV, Dijkstra, fragmentation, masks/subnets, and CIDR. Then practice explaining *why* each mechanism exists. Finally test definitions and header fields from memory. This is a study recommendation based on the review's emphasis, not an official weighting.

## 1. Internet foundations

### Internet, protocols, and components

**Internet:** A network of interconnected networks. A home or institution connects through an access ISP; ISPs interconnect through other providers, peering links, and Internet exchange points. Content providers can operate their own networks and connect near users.

- **Host / end system:** Runs applications, such as a client or server.
- **Link:** A communication medium such as copper, fiber, or radio.
- **Switch/router:** Forwards traffic between links.
- **Protocol:** Specifies message formats, message order, and the actions taken on sending, receiving, or other events.
- **ISP:** Provides network connectivity. A provider-customer relationship and a peering relationship are different ways networks interconnect.
- **IXP:** A location/infrastructure where networks can exchange traffic.
- **RFC / IETF:** Internet specifications are published as Requests for Comments; the Internet Engineering Task Force develops many Internet standards. Not every RFC is itself a standard.

The Internet can also be described as a service infrastructure: applications use a programming interface to exchange data without implementing every link and routing mechanism themselves.

**Why not directly connect every pair of hosts or ISPs?** With $n$ participants, a full mesh needs $n(n-1)/2$ links, growing as $O(n^2)$. Shared links and interconnected switching networks scale better.

### Circuit switching versus packet switching

| | Circuit switching | Packet switching |
| --- | --- | --- |
| Resource allocation | Reserves capacity along a path for a call | Traffic shares links as packets arrive |
| Idle sender | Reserved capacity stays unavailable to other calls | Other packets can use otherwise idle capacity |
| Performance | Predictable capacity after successful reservation | Delay and throughput depend on sharing and congestion |
| Congestion consequence | A new call may not obtain a circuit | Packets wait, or are dropped if buffers fill |

**Circuit segment idle:** Suppose a call reserves 1 Mbps on each link of its path but currently has no data. That capacity remains reserved even though it carries nothing. Another call cannot borrow that reserved segment in the basic circuit model.

A circuit does **not** necessarily reserve an entire physical link. Several circuits can occupy different portions of one link at the same time. Therefore, “single-threaded versus multithreaded” is not an accurate distinction.

**Packet switching:** A sender divides data into packets. Packets from different flows take turns on a shared output link. Each transmitted packet can use the link's full transmission rate during its turn; a flow's average throughput can be much lower.

**Multiplexing:** Sharing one physical resource among multiple logical flows. Both circuit switching and packet switching can multiplex flows, but the resource-allocation rules differ.

### Switches, routers, edge, and core

- A typical **link-layer switch** connects devices within a LAN and forwards frames using link-layer addresses, such as Ethernet MAC addresses.
- A **router** connects IP networks and forwards packets using destination IP prefixes and a forwarding table.
- The **edge** contains hosts and access networks: home Ethernet/Wi-Fi, enterprise networks, and cellular access.
- The **core** is the interconnected router network carrying packets between networks.

**Forwarding versus routing:** Forwarding is the local decision, “Which output link should carry this arriving packet?” Routing is the distributed process that determines paths and produces the information used by forwarding tables. Computing routes and forwarding user traffic are separate activities.

## 2. Delay, loss, bandwidth, and throughput

### Four delay components

$$d_{\text{nodal}}=d_{\text{proc}}+d_{\text{queue}}+d_{\text{trans}}+d_{\text{prop}}$$

| Component | Meaning | Main dependency |
| --- | --- | --- |
| Processing | Check a packet and select its output link | Router processing work |
| Queueing | Wait before transmission | Traffic and congestion |
| Transmission | Put all packet bits onto the link | Packet size and link rate |
| Propagation | Travel across the physical link | Distance and signal speed |

$$d_{\text{trans}}=\frac{L}{R},\qquad d_{\text{prop}}=\frac{d}{v}$$

Here $L$ is **bits**, $R$ is **bits/second**, $d$ is distance, and $v$ is propagation speed. Convert bytes to bits before using $L/R$.

**Worked example:** A 1500-byte packet crosses a 100 Mbps link that is 1000 km long, with propagation speed $2\times10^8$ m/s.

- Transmission: $1500\times8/(100\times10^6)=0.00012$ s = **0.12 ms**.
- Propagation: $10^6/(2\times10^8)=0.005$ s = **5 ms**.
- With negligible processing and queueing, the last bit arrives after **5.12 ms**.

Increasing bandwidth reduces transmission delay, but it does not make the signal travel faster. Making a packet smaller also reduces transmission delay, not propagation delay.

For a path, add the relevant delays at each hop. If a question specifies **store-and-forward**, a router must receive the whole packet before sending it onward. For one packet across $k$ equal-rate links, with no processing/queueing, the delay is $kL/R+\sum_i d_i/v_i$. Multiple packets can overlap on different links, so do not confuse one-packet latency with sustained throughput.

### Bandwidth versus throughput

**Bandwidth:** A link's transmission capacity, measured here in bits/second.

**Throughput:** The achieved transfer rate between sender and receiver. It can be instantaneous or averaged over a period.

An otherwise idle path with link rates $R_1,\ldots,R_k$ has ideal sustained throughput limited by:

$$T\leq\min_i R_i$$

If $m$ connections fairly share a bottleneck link of rate $R$, each gets at most approximately $R/m$. Include the endpoint access links too:

$$T\approx\min(R_s,R_c,R/m)$$

**Example:** Server access is 20 Mbps, client access is 8 Mbps, and ten flows fairly share a 50 Mbps backbone. Per-flow throughput is approximately $\min(20,8,5)=5$ Mbps. A 100 Mbit file takes about $100/5=20$ seconds, ignoring startup and protocol overhead.

### Queueing and loss

If packets arrive faster than an output link can send them, a queue grows. Temporary bursts can cause queueing even if the long-term average load is low.

Buffers are finite. An arriving packet may be dropped when the buffer is full. IP's best-effort service does not itself guarantee retransmission; another protocol or the application must handle recovery if needed.

**Common mistake:** More bandwidth, less delay, no loss, and reliable delivery are different properties. Improving one does not automatically guarantee the others.

## 3. Layering and encapsulation

### Internet stack and OSI model

| Internet layer | Service | Examples / data unit |
| --- | --- | --- |
| Application | Application-level exchanges | HTTP, SMTP, FTP; message |
| Transport | Process-to-process delivery | TCP/UDP; segment or datagram |
| Network | Delivery across networks | IP/routing; packet or IP datagram |
| Link | Delivery to a neighboring interface | Ethernet/Wi-Fi; frame |
| Physical | Transmission over the medium | Bits |

The **OSI seven-layer model**, bottom to top, is physical, data link, network, transport, **session**, **presentation**, application.

- **Session:** Organizes related exchanges or transport streams belonging to a session.
- **Presentation:** Handles representation, encoding, and related transformations such as encryption.
- The Internet model does not have separate session/presentation layers; applications and their supporting protocols handle those functions.

**Why layer?** Each layer exposes a service to the layer above and uses the service below. Implementations can change without redesigning every other layer, provided the interfaces remain compatible.

**Possible downside:** Strict boundaries can duplicate work, such as error recovery at multiple layers, or conceal information another layer could use. Layering helps modularity but is not free of tradeoffs.

### Encapsulation and intermediate devices

The application supplies a message. Transport adds its header; IP adds an IP header; the link layer wraps the result in a frame. The receiver removes the corresponding information in the reverse order. Ethernet also has a trailer, so “every layer just adds a header” is a useful simplification rather than a universal rule.

- **End hosts** normally process the whole stack.
- A typical **switch** processes physical/link-layer information.
- A typical **router** processes through the network layer for forwarding. It removes the incoming link framing and creates suitable framing for the next link.

The IP payload is commonly a TCP segment or UDP datagram, but it can also be another protocol. For example, OSPF runs directly over IP. Do not assume every control protocol must use TCP or UDP.

## 4. Distance-vector routing

### Graph model and router state

Model the network as $G=(N,E)$: routers are nodes, links are edges, and each edge has a cost. Path cost is the sum of its link costs. A cost might represent hop count, inverse bandwidth, or congestion, depending on the protocol.

Static routes cannot automatically handle failures, added links/nodes, or changed costs. Dynamic routing exchanges information and updates routes as conditions change.

In **distance vector (DV)**, each router knows:

1. Its direct-neighbor link costs.
2. Its own current distance estimates and next hops.
3. In the lecture's full implementation, the latest distance vector from **each** neighbor.

It does not need a complete topology map or the full sequence of routers on each route.

Initialization: cost to self is 0, direct neighbors have their link costs, and unknown destinations have cost $\infty$. A routing entry is `<Destination, Cost, NextHop>`.

**Storage versus update timing:** With n destinations and d neighbors, storing all neighbor vectors takes $O(dn)$ space, plus the router's own $O(n)$ table. Keeping only the best entries uses less state but loses alternatives. Periodic versus triggered transmission does not inherently change these bounds; explain which information an implementation retains before claiming that an update policy requires more or less space. This distinction matters for the 2015 routing question.

### Bellman-Ford update

$$D_x(y)=\min_{v\in N(x)}\{c(x,v)+D_v(y)\}$$

- $c(x,v)$ is the direct cost from $x$ to neighbor $v$.
- $D_v(y)$ is that neighbor's most recently advertised cost to destination $y$.
- Add the cost to reach the neighbor, then compare all neighbors.
- The minimizing neighbor becomes the next hop.

**Example:** Neighbors of $u$ are $v,x,w$, with link costs $2,1,5$. They advertise costs to $z$ of $5,3,3$ respectively:

$$D_u(z)=\min(2+5,1+3,5+3)=4$$

The resulting entry is `<z, 4, x>`. The next hop is $x$, not $z$, and 4 is the **whole path cost**, not the direct-link cost.

### Lecture graph: one DV round

Use the undirected links from deck 3: `A-B=19`, `A-C=7`, `B-C=11`, `B-D=4`, `C-D=15`, `C-E=5`, `D-E=13`.

After receiving neighbors' **initial** vectors, A calculates:

| Destination | Via B | Via C | Best entry at A |
| --- | ---: | ---: | --- |
| B | $19+0=19$ | $7+11=18$ | `<B, 18, C>` |
| C | $19+11=30$ | $7+0=7$ | `<C, 7, C>` |
| D | $19+4=23$ | $7+15=22$ | `<D, 22, C>` |
| E | $19+\infty$ | $7+5=12$ | `<E, 12, C>` |

For an exam's synchronous “rounds,” compute round $k+1$ using advertisements from round $k$. Do not silently use another router's newly computed value in the same round. Real DV can operate asynchronously, so a question specifying an event order must follow that order instead.

### Advertisements and convergence

**Triggered update:** Send an advertisement when routing information changes, for example after a link failure, link-cost change, or a received vector that changes the local route.

**Periodic update:** Send routing information on a timer even when the best routes have not changed. RIP commonly uses about 30 seconds.

Periodic updates serve more than one purpose:

- Refresh soft-state information so it does not expire while still valid.
- Allow a neighbor to detect a router that has stopped responding.
- Repair information lost with an earlier advertisement.
- Re-advertise alternatives after another router's situation changes, even if the advertising router's own best route did not change.

**Why keeping neighbor vectors matters:** In a triangle with all edges costing 1, B's direct route to C stays at 1 when A-C fails. If A retained only its previous best direct route to C and discarded B's old advertisement, B might have no changed route to trigger an update. A needs a refresh or another mechanism to learn the still-valid route through B. With B's vector already stored, A can immediately recompute $1+1=2$.

**Convergence:** Routers' estimates stabilize to the correct routes after information propagates and the topology stops changing. A local update does not instantly update every router.

If asked whether triggered updates are “necessary,” state the assumptions. Periodic updates alone can eventually propagate changes under suitable delivery/failure-detection assumptions; triggered updates improve reaction time. Event-only updates need other mechanisms for lost messages and stale state.

### Count to infinity: the lecture's X-Y-Z example

Initially `X-Y=4`, `Y-Z=1`, and `X-Z=50`. Y reaches X directly at cost 4. Z reaches X through Y at cost 5.

Now X-Y increases to 60. Y has learned the new **local** cost, but still has Z's old advertisement, “I reach X at cost 5.”

| Event | Y's estimate to X | Z's estimate to X |
| --- | --- | --- |
| Before change | 4, next hop X | 5, next hop Y |
| Y recomputes with Z's stale vector | $\min(60,1+5)=6$, via Z | Still 5, via Y |
| Z receives Y's new advertisement | Still 6, via Z | $\min(50,1+6)=7$, via Y |
| Y receives Z's new advertisement | $\min(60,1+7)=8$, via Z | Still 7, via Y |
| Later updates | Estimates continue rising | Eventually uses direct X at 50 |
| Stable result | 51, via Z | 50, via X |

**The 6 is Y's estimated path cost to X, not the cost of Y-Z.** Y-Z remains 1. Y cannot see that Z's old cost 5 was obtained through Y itself, so each treats the other's dependent estimate as an independent escape route. Packets can loop between them.

This example stops counting when Z switches to its real direct route at 50, after which Y learns a valid route of 51. Without any alternative path, estimates can increase toward infinity or the protocol's finite “unreachable” value. Cost decreases typically spread much more straightforwardly because a genuinely better route can propagate outward.

### Split horizon, poisoned reverse, and RIP

| Technique | What Z tells Y if Z reaches X through Y | Benefit / limitation |
| --- | --- | --- |
| Ordinary DV | Advertises its finite cost to X | Y may reuse a route that depends on itself |
| Split horizon | Omits that route from advertisements to Y | Does not feed the learned route back, but stale cached information may remain |
| Poisoned reverse | Explicitly advertises X as unreachable to Y | Invalidates Y's apparent alternative through Z |

The restriction applies to the neighbor used as next hop. Z can still advertise its finite route to other neighbors.

Deck 4 illustrates why omission and explicit poisoning differ: after B fails, C and D can both retain old routes to A through B. With split horizon, both may select the other using stale information and then omit further updates to that neighbor. Poisoned reverse explicitly communicates that the dependent route is unusable through the advertising router.

**Limit:** Poisoned reverse addresses mutual two-router dependencies; it does not solve every loop involving three or more routers. Avoid claiming that it makes all DV routing loop-free.

**RIP (Routing Information Protocol):** A DV implementation using hop count. Costs 0-15 represent reachable distances; **16 means unreachable**. A small infinity bounds count-to-infinity delay but excludes destinations requiring more than 15 hops. Hold-down/aging mechanisms can reduce instability or remove stale information, but do not memorize an unspecified timer value.

## 5. Link-state routing and Dijkstra

### Information distribution

**Link-state routing:** Each router advertises its own direct-neighbor links and costs. Flooding distributes those local descriptions throughout the routing domain. Combining them builds a topology map.

Each router does **not** need to send a whole graph in each LSP. The graph comes from the collection of all routers' LSPs.

After convergence, routers in the same relevant flooding domain have a consistent topology database. During changes, they may temporarily have different versions. This is not a guarantee that every router on the entire Internet has one identical global graph.

### LSP fields and reliable flooding

An **LSP (Link-State Packet)** contains:

1. **Originating router ID:** Whose links this description represents.
2. **Neighbors and link costs:** The router's local connectivity.
3. **Sequence number:** Distinguishes newer information from old or duplicate information from that same origin.
4. **Lifetime/TTL:** Allows stale information to expire even if no replacement arrives.

**Receiver's procedure:**

1. If there is no stored LSP for that origin, accept it.
2. Otherwise compare versions. Accept newer information; ignore older or duplicate copies for database replacement and re-flooding.
3. Store accepted information and forward it to other neighbors, excluding the incoming neighbor.
4. Recompute shortest paths when the topology information changes.x
5. Age stored information and remove it when its lifetime expires. Originators periodically refresh their advertisements.

**Duplicate example:** B receives the same A-originated LSP through C and D. It accepts the first new copy. The second copy with the same sequence number does not start another flooding wave. Version checks prevent uncontrolled circulation.

**Sequence number versus TTL:** Sequence numbers answer “Which version is newer?” Lifetime answers “Is this information still valid?” Lifetime is needed if the origin fails and can no longer send a new version. Restarting sequence numbers can make new information look older than surviving copies; the lecture's simplified restart-at-zero model therefore depends on stale-information removal or restart handling.

**LSP lifetime versus IPv4 TTL:** LSP aging is time-based, including while stored in a database. IPv4 TTL functions as a hop budget decremented at routers. The lecture also adjusts LSP lifetime during flooding. OSPF concretely represents LSA age in **seconds**, increasing it in the database and for estimated transmission delay. [OSPF specification, sections 12.1.1 and 14](https://www.rfc-editor.org/rfc/rfc2328.html#section-12.1.1)

### Dijkstra's algorithm

With the topology database, each router independently runs Dijkstra with **itself as source**. Non-negative link weights allow the smallest tentative distance to be finalized.

- **Confirmed / M:** Nodes with final shortest-path costs.
- **Tentative:** Best discovered candidates not yet final.
- **Relaxation:** Test whether a route through a newly confirmed node is cheaper.

```text
M = {s}; C(s) = 0
For each n != s: C(n) = direct_cost(s, n), or infinity
While an unconfirmed reachable node remains:
    choose w outside M with minimum C(w)
    add w to M
    for each unconfirmed neighbor n of w:
        C(n) = min(C(n), C(w) + link_cost(w, n))
```

Record predecessors or first hops as costs improve. If a better route to n goes through w, n inherits the **first hop from s toward w**, except that a direct route from s starts with n itself. w need not be the next hop at s.

### Dijkstra on the same lecture graph

Starting at A, use the graph from Section 4:

| Newly confirmed | Remaining tentative entries `<destination, cost, next hop>` |
| --- | --- |
| A, cost 0 | `<C, 7, C>`, `<B, 19, B>` |
| C, cost 7 | `<E, 12, C>`, `<B, 18, C>`, `<D, 22, C>` |
| E, cost 12 | B stays 18; D stays 22 because $12+13=25$ is worse |
| B, cost 18 | D stays 22 because $18+4=22$ ties its existing cost |
| D, cost 22 | Complete |

Final order: **A, C, E, B, D**. A's costs are B=18, C=7, D=22, E=12, and every remote destination uses first hop C. D has equal-cost full paths `A-C-D` and `A-C-B-D`; the first hop is C in either case.

The straightforward implementation scans candidates and takes $O(n^2)$ time. A priority queue with adjacency lists can improve the bound to $O((n+E)\log n)$ using a binary heap. This data-structure comparison is extra practice prompted by the 2015 exam; the main skill is correctly tracing selection and relaxation.

### LS versus DV and OSPF

| | Distance vector | Link state |
| --- | --- | --- |
| Advertised content | Cost estimates to destinations | Own direct-neighbor links and costs |
| Distribution | Immediate neighbors | Flooding throughout the domain |
| Local knowledge | Neighbor estimates and own next hops | Topology database plus computed next hops |
| Algorithm | Distributed Bellman-Ford updates | Local Dijkstra after information distribution |
| Convergence concerns | Dependent stale estimates, loops, count to infinity | Flooding delay, inconsistent temporary databases, possible oscillations |
| Work/control traffic | Relatively simple local calculation; repeated exchanges | Topology storage, shortest-path calculation, flooding |
| Faulty advertisement | Incorrect path cost feeds other routers' calculations | Incorrect local-link claim corrupts the topology description |

For one set of advertisements from all $n$ origins, flooding traversing the links gives a lecture-level $O(nE)$ message bound. DV's total message count depends on how many exchanges convergence requires. Do not present LS as always faster or cheaper in every network.

**OSPF (Open Shortest Path First):** An implementation of link-state routing. It floods link-state advertisements and computes shortest paths. It runs directly over IP with **protocol number 89**, rather than inside TCP or UDP. Understand that OSPF is a routing protocol and IP carries user/control packets using the resulting forwarding information.

## 6. IPv4 service and header

### What IP provides

**Internet Protocol:** A common network-layer packet and addressing system that lets different underlying networks operate as one internetwork.

**Connectionless, best-effort service:** Each datagram is handled independently. IP does not guarantee arrival, arrival order, no duplicates, or a bounded delay. Higher-layer protocols or applications supply any needed reliability.

The network-layer roles shown in deck 5 are:

- **Routing protocols:** Select paths and maintain routing information.
- **IP:** Defines addressing, datagram format, and forwarding/packet-handling conventions.
- **ICMP:** Reports network errors and control information. Its detailed mechanisms are beyond the pre-NAT cutoff.

### Header fields to recognize

Without options, an IPv4 header is **20 bytes**. Header length is variable, and the total-length field includes both header and payload.

| Field | Purpose / interpretation |
| --- | --- |
| Version | 4 for IPv4 |
| IHL / header length | Header size in **4-byte words**; IHL 5 means 20 bytes |
| Type of service | Traffic-treatment information |
| Total length | Entire IP datagram size in bytes, maximum 65,535 |
| Identification | Distinguishes datagrams for fragmentation/reassembly |
| Flags | Includes DF, “don't fragment,” and MF, “more fragments” |
| Fragment offset | Payload position in the original datagram, in **8-byte units** |
| TTL | Limits router hops, preventing indefinite circulation |
| Protocol | Identifies the next protocol, such as TCP, UDP, or OSPF |
| Header checksum | Detects errors in the IP header, excluding payload |
| Source/destination | 32-bit interface addresses |
| Options | Optional header information; increases header length |

$$\text{payload bytes}=\text{total length}-4\times\text{IHL}$$

Routers change TTL, so they must update the IPv4 header checksum. Payload integrity is the responsibility of other layers. An IP protocol number selects a protocol; it is different from a TCP/UDP port that selects an application endpoint.

## 7. IPv4 fragmentation and reassembly

### Why and where fragmentation happens

**MTU:** The largest IP packet a link can carry, including its IP header but excluding link-layer overhead in these problems.

An IPv4 packet larger than the outgoing MTU may be fragmented by a router if fragmentation is allowed. With DF=1, a router cannot solve the size problem by fragmenting the packet. A larger later MTU does not cause a router to reassemble existing fragments; **reassembly occurs at the final destination**.

Each fragment is an IP packet with its own header. Fragmentation adds overhead and creates more packets whose loss can prevent completion of the original datagram.

### Calculation recipe

Let original total length be $T$, header length $H$, and outgoing MTU $M$.

1. Compute original payload: $P=T-H$.
2. For maximum-size non-final fragments, compute payload capacity:

$$Q=8\left\lfloor\frac{M-H}{8}\right\rfloor$$

3. Split the payload into chunks of Q bytes, plus any remainder. Every **non-final fragment of the original datagram** needs a payload length divisible by 8.
4. Each fragment's total length is its own payload length plus H.
5. Its offset is the number of original payload bytes before it, divided by 8.
6. Preserve the datagram's identification. Set MF=1 if more original payload follows; MF=0 only on the final fragment of the original datagram.

Assume no options unless specified. Options can complicate header copying, so use the problem's stated header lengths rather than applying H=20 blindly.

### Worked lecture example

Original total length **1420**, header **20**, payload **1400**, MTU **532**:

$$Q=8\lfloor(532-20)/8\rfloor=512$$

| Fragment | ID | Payload bytes | Total length | Offset | MF |
| --- | --- | ---: | ---: | ---: | ---: |
| 1 | Same original ID | 512 | 532 | 0 | 1 |
| 2 | Same original ID | 512 | 532 | 64 | 1 |
| 3 | Same original ID | 376 | 396 | 128 | 0 |

Check: payloads sum to 1400. Offsets 64 and 128 mean byte positions 512 and 1024, not positions 64 and 128.

### Re-fragmenting an existing fragment

Suppose a received fragment has ID=32317, **offset=400**, **MF=0**, **DF=0**, and payload=300 bytes. It must cross an MTU=276 link, with 20-byte headers. This matches the arithmetic pattern in the 2017 exam.

Capacity is $276-20=256$ payload bytes. The original fragment starts at byte $400\times8=3200$ of the original datagram.

| New fragment | Payload | Total length | Offset | MF | DF |
| --- | ---: | ---: | ---: | ---: | ---: |
| First | 256 | 276 | 400 | 1 | 0 |
| Second | 44 | 64 | $400+256/8=432$ | 0 | 0 |

Both retain ID=32317. **Offsets remain relative to the original datagram**, so do not restart at zero. If the input fragment had MF=1, its final child would also have MF=1 because more original data still follows. For a valid MF=1 input, its payload must also satisfy the multiple-of-8 rule.

TTL is a separate forwarding operation: if the input TTL is 17 before the router forwards it, the outgoing fragments have TTL 16. If a question gives the already-decremented outgoing TTL, do not decrement again. State which interpretation you use.

### Reassembly questions from the slides

- **Which fragments belong together?** Use source IP, destination IP, protocol, and identification, not identification alone across all traffic.
- **Where does a fragment's data go?** At byte position $8\times\text{offset}$ in the original payload buffer.
- **How is the end found?** A fragment with MF=0 establishes the end at $8\times\text{offset}+\text{payload length}$.
- **When is reassembly complete?** When the final size is known and every byte from the beginning through that end has arrived. Receiving the last fragment alone is insufficient.
- **How is an unfragmented packet identified?** Offset=0 and MF=0. DF alone does not indicate whether a received packet is a fragment.
- **What if fragments never all arrive?** A reassembly timer expires and the incomplete datagram is discarded; the buffer cannot be retained forever.
- **Which header fields change in the reconstructed packet?** Total length represents the whole datagram again, offset becomes 0, MF becomes 0, and the header checksum must correspond to the reconstructed header. There is one resulting header rather than concatenated fragment headers.

**Malformed-fragment check:** Offset=8191 starts at byte $8191\times8=65,528$. If payload length is 1004, the required payload end would be 66,532, beyond IPv4's maximum total datagram size even before adding its header. Reject the malformed input rather than allocating an arbitrarily large reassembly buffer. Changing MF does not make that oversize valid.

**Common mistakes:** Subtracting no header from the MTU; rounding payload upward; using byte offsets instead of 8-byte offsets; treating MF=0 as “unfragmented”; setting a re-fragmented MF=1 parent's last child to MF=0; reassembling at every router.

## 8. IPv4 addressing and historical classes

### Interfaces and hierarchical addresses

An IPv4 address has **32 bits**, written as four decimal octets, each 0-255. An address identifies an **interface**, so a router or multihomed host can have multiple addresses.

Split an address into a network prefix and a host portion. Routers can then direct an entire block toward a network using one entry instead of keeping a separate route to every host. Adding a host inside an existing block does not normally require a new Internet-wide prefix route.

Address allocation is hierarchical: ICANN/IANA-level allocation, regional registries, ISPs/institutions, then customer networks. The important idea is allocation of blocks that can be subdivided and aggregated, not the decks' dated count of global prefixes.

### Classful addressing

Historically, leading bits determined an address's class and default network/host split.

| Class | Leading bits | First-octet range | Default mask | Host bits | Ordinary usable hosts per network |
| --- | --- | --- | --- | ---: | ---: |
| A | `0` | Pattern covers 0-127; ordinary historical networks use 1-126 | `255.0.0.0` = /8 | 24 | $2^{24}-2=16,777,214$ |
| B | `10` | 128-191 | `255.255.0.0` = /16 | 16 | $2^{16}-2=65,534$ |
| C | `110` | 192-223 | `255.255.255.0` = /24 | 8 | $2^8-2=254$ |
| D | `1110` | 224-239 | Multicast; no ordinary network/host split | - | - |
| E | `1111` | 240-255 | Reserved; no ordinary network/host split | - | - |

`0` and `127` have special uses, so they are excluded from ordinary historical Class A network allocations. There are 126 such A networks, $2^{14}$ Class B networks, and $2^{21}$ Class C networks.

Leading-bit patterns occupy **1/2** of the 32-bit space for A, **1/4** for B, and **1/8** for C; D and E each occupy **1/16**. These are fractions of the address space, not fractions of today's networks or Internet users.

**Source correction:** Deck 6's text gives Class E as `11110*`, but its diagram and the standard use **`1111`**. [IPv4 multicast/address-class specification](https://www.rfc-editor.org/rfc/rfc1112.html#section-4)

Examples under classful defaults: `10.3.2.4` is A with network `10.0.0.0`; `128.96.33.81` is B with network `128.96.0.0`; `192.12.69.77` is C with network `192.12.69.0`.

**Why classful allocation was wasteful:** An organization needing 300 hosts cannot fit in one Class C block of 254 usable addresses; a Class B allocation is vastly larger than needed. Likewise, 65,536 usable hosts do not fit in Class B, which provides 65,534. Always distinguish all addresses from usable host addresses.

## 9. Subnetting, CIDR, and forwarding

### Prefix, mask, AND, and broadcast

For `/p`, the leftmost p bits are network bits and the remaining $h=32-p$ bits are host bits. The mask has p leading 1s followed by h zeros.

$$\text{network address}=\text{IP address}\mathbin{\&}\text{subnet mask}$$

Bitwise AND keeps a bit only where both operands have 1. The mask preserves network bits and clears host bits. This operation determines which network an address belongs to so a host/router can choose direct delivery or a next hop.

**Worked example from the 2015 exam:** Host `128.8.129.2`, mask `255.255.254.0` = `/23`.

```text
Third octet of IP:    129 = 10000001
Third octet of mask:  254 = 11111110
AND result:          128 = 10000000
```

- Network: **128.8.128.0/23**.
- Broadcast: **128.8.129.255**, obtained by setting all nine host bits to 1.
- Total addresses: $2^9=512$.
- Ordinary usable hosts: **510**, from `128.8.128.1` through `128.8.129.254`.
- A possible first-hop router: **128.8.128.1**, if assigned to the router. It must be on the subnet and distinct from the host/network/broadcast addresses; there is no uniquely inferable gateway from the mask alone.

For ordinary subnets with host and broadcast addresses:

$$\text{total addresses}=2^{32-p},\qquad\text{usable hosts}=2^{32-p}-2$$

The subtraction accounts for the all-zero host part (network address) and all-one host part (broadcast). Apply this convention to these course problems. `/31` point-to-point and `/32` host-route special cases should not be treated with the ordinary subtract-two formula.

### Fast mask and block-size reference

| Prefix | Mask | Total addresses | Ordinary usable hosts |
| --- | --- | ---: | ---: |
| /21 | 255.255.248.0 | 2048 | 2046 |
| /22 | 255.255.252.0 | 1024 | 1022 |
| /23 | 255.255.254.0 | 512 | 510 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |

Partial mask-octet values are **128, 192, 224, 240, 248, 252, 254** for 1-7 leading bits. In the varying octet, block increments are $256-\text{mask octet}$: /23 blocks start at even third octets; /26 blocks start at fourth octets 0, 64, 128, 192.

**Alignment:** `44.100.101.0/23` describes the block **44.100.100.0/23**, not a block starting at third octet 101. Normalize an address using its mask before allocating subnets. The 2021 exam uses the noncanonical spelling.

### Borrowing bits and choosing subnet sizes

Borrowing b bits from an original prefix creates $2^b$ equal subnets. The new prefix is `/p+b`, leaving fewer hosts per subnet.

**Equal split:** Divide `128.8.128.0/23` into four equal subnets. Borrow 2 bits, giving `/25` and mask `255.255.255.128`:

- `128.8.128.0/25`
- `128.8.128.128/25`
- `128.8.129.0/25`
- `128.8.129.128/25`

Each has 128 addresses and 126 ordinary usable host addresses.

**Unequal split:** Divide `200.23.16.0/23` into one large subnet and two half-sized subnets. Allocate one /24 and two /25s:

| Subnet | Mask | Broadcast | First usable | Last usable | Usable hosts |
| --- | --- | --- | --- | --- | ---: |
| 200.23.16.0/24 | 255.255.255.0 | 200.23.16.255 | 200.23.16.1 | 200.23.16.254 | 254 |
| 200.23.17.0/25 | 255.255.255.128 | 200.23.17.127 | 200.23.17.1 | 200.23.17.126 | 126 |
| 200.23.17.128/25 | 255.255.255.128 | 200.23.17.255 | 200.23.17.129 | 200.23.17.254 | 126 |

The blocks have sizes 256, 128, and 128; the **usable-host counts** are not exactly in a 2:1 ratio because each subnet reserves two addresses.

**Host requirements:** Choose the smallest h satisfying $2^h-2\geq H$, where H is the required number of interface addresses. For 30, 22, and 18 hosts, each needs at least 5 host bits, so each can use a /27. In `200.23.16.0/24`, valid allocations are `.0/27`, `.32/27`, and `.64/27`. Include a gateway address in H if the question counts only end hosts but also requires a router interface.

The host portion of the parent block is now split into **subnet bits plus remaining host bits**. A /24 split into /27s uses 3 new subnet bits and leaves 5 host bits.

### Forwarding with subnet masks

A table entry `<SubnetNum, SubnetMask, NextHop>` matches a destination D if:

$$D\mathbin{\&}\text{SubnetMask}=\text{SubnetNum}$$

If the chosen next hop is a local interface, deliver on that connected network. Otherwise send to the selected router. If no specific route matches, use a default route if available; without a matching/default route, there is no forwarding path.

The lecture table has `128.96.34.0/25` on Interface 0, `128.96.34.128/25` on Interface 1, and `128.96.33.0/24` through R2. Thus `.34.42` uses Interface 0, `.34.200` uses Interface 1, and `.33.80` uses R2.

For a simple host, compare the destination's network under the local mask with the host's own network. Same subnet means direct delivery; otherwise use the configured gateway. Merely sharing the first three decimal octets does not establish subnet membership.

Deck 6 notes that older subnetting can use noncontiguous mask bits and that multiple IP subnets can share one physical network. **CIDR `/p` prefixes specifically use contiguous leading bits**, so use the contiguous-mask arithmetic above for CIDR questions. The historical noncontiguous-mask distinction is documented in [RFC 950](https://www.rfc-editor.org/rfc/rfc950.html#section-2.1).

### CIDR and route aggregation

**CIDR (Classless Inter-Domain Routing):** Uses an explicit prefix length instead of inferring /8, /16, or /24 from the address class. `200.23.16.0/23` includes `200.23.16.0` through `200.23.17.255`.

- **Subnetting:** Divide an allocation into smaller blocks with longer prefixes.
- **Aggregation / supernetting:** Summarize blocks using a shorter common prefix.
- Hierarchical aggregation reduces forwarding-table size and can hide an organization's internal subdivisions from outside routers.

**Exact aggregation needs alignment and coverage.** To replace several blocks with one prefix covering exactly their addresses, the combined size must be a power of two, the start must align with that size, and the blocks must fill the aggregate without holes.

**Worked old-exam pattern:** `112.8.32.0/24`, `.33.0/24`, `.34.0/24`, and `.35.0/24` aggregate to **112.8.32.0/22**. The third octets share their first six bits:

```text
32 = 00100000
33 = 00100001
34 = 00100010
35 = 00100011
```

The first two octets contribute 16 bits, plus six shared third-octet bits, making /22. A /22 begins at a multiple of 4 in the third octet, and 32 is aligned.

The 2015 aggregation example, `118.20.224.0/22`, `.228.0/23`, and `.230.0/23`, fills **118.20.224.0/21**, covering third octets 224-231.

**Counterexample:** `200.20.1.0/24`, `.2.0/24`, and `.3.0/24` fit inside `200.20.0.0/22`, but that summary also includes `.0.0/24`. An exact representation is `.1.0/24` plus `.2.0/23`. A broader summary requires responsibility for, or explicit handling of, the extra space; do not assume neighboring-looking addresses always form an exact aggregate.

### Longest-prefix matching

When several table entries match, choose the one with the **largest prefix length**. A default route `0.0.0.0/0` matches everything but is least specific.

| Entry | Output | Does 201.10.6.17 match? |
| --- | --- | --- |
| 0.0.0.0/0 | Default | Yes |
| 201.10.0.0/21 | Provider 1 | Yes |
| 201.10.6.0/23 | Provider 2 | Yes; **selected** |

The /23 exception overrides the /21 summary. This supports cases where a customer has another provider or a specific subnet needs different forwarding. Prefix selection is not “pick the first row,” “pick the lowest cost among all matching prefixes,” or “pick the numerically largest network address.”

## 10. DHCP and relay agents

### DHCP's purpose and DORA

**DHCP (Dynamic Host Configuration Protocol):** Automatically supplies a host's network configuration. A server allocates addresses from a pool using renewable leases, allowing addresses to be reused instead of permanently assigned to absent devices.

DHCP can supply an IP address, subnet mask, default gateway, DNS-server information, and lease duration. The mask identifies the local subnet; the gateway handles off-subnet traffic; DNS supports name resolution. These are different functions.

The standard fresh-allocation sequence is **DORA**:

| Step | Message | Meaning |
| --- | --- | --- |
| D | DHCPDISCOVER | Client asks whether a server is available |
| O | DHCPOFFER | Server proposes an address and configuration |
| R | DHCPREQUEST | Client selects/requests an offer |
| A | DHCPACK | Server confirms the lease |

A new client can initially use source **0.0.0.0** and destination **255.255.255.255**, the limited broadcast address, because it does not yet have a usable address or know its server. DHCP uses UDP: client port **68**, server port **67**, as shown in the lecture diagram.

In the diagram, offers/ACKs also use broadcast delivery. Do not generalize this into “all DHCP traffic is always broadcast”; relay traffic and some later exchanges use unicast. A lease renewal also need not repeat the entire fresh-allocation discovery sequence.

### Why a relay is needed

Routers normally do not forward local broadcasts to other networks. Without a relay, a client's broadcast cannot find a DHCP server on another subnet.

1. The client broadcasts its discovery on its own subnet.
2. A **DHCP relay agent** receives it and forwards it to the remote server using unicast, identifying the client's subnet for address selection.
3. The server offers configuration from the appropriate address pool and returns the response through the relay.
4. The relay forwards it to the client. The request/acknowledgment exchange is relayed as well.

One server can therefore serve multiple subnets, provided clients have a local server or a relay path. The relay forwards DHCP messages; it does not become a second independent address allocator.

**Broadcast distinction:** `255.255.255.255` is a limited local broadcast. A subnet's broadcast, such as `128.8.129.255` for `128.8.128.0/23`, is calculated from that subnet's mask. They are not interchangeable names for the same address.

For the configuration screenshots in deck 7, be able to identify the interface IP, mask, and gateway; use them to derive the network and broadcast. `ipconfig` is the Windows example and `ifconfig` is the Linux/macOS example shown in the slides.

## 11. Practice questions

These questions are original or adapted from the supplied material. They are a self-test, not a prediction of the current exam. Solve them before reading Section 12.

1. A circuit reserves 2 Mbps on a 10 Mbps link. Its sender becomes idle. Can another circuit use that reserved 2 Mbps in the basic model? Does the first circuit block all 10 Mbps?
2. A 1000-byte packet crosses a 10 Mbps, 200 km link. Propagation speed is $2\times10^8$ m/s. Find transmission and propagation delay. Which changes if the link rate doubles?
3. A sender access link is 12 Mbps, a receiver access link is 9 Mbps, and four flows fairly share a 20 Mbps middle link. Find ideal per-flow throughput and the transfer time for a 40 Mbit file.
4. List the Internet layers in order from top to bottom. Which two extra layers appear in the OSI model? Which layers does a normal forwarding router need?
5. X has neighbors A and B. Link costs are X-A=3 and X-B=6. A advertises destination Z at 8; B advertises it at 2. Compute X's route. Then B advertises Z at 20. Recompute using the stored vector from A.
6. In the lecture's X-Y-Z example, explain why Y's cost to X becomes 6 after X-Y becomes 60. What does Z advertise to Y with poisoned reverse? Does poisoned reverse eliminate all loops?
7. Run Dijkstra from A on `A-B=2`, `A-C=5`, `B-C=1`, `B-D=4`, `C-D=1`. Show confirmation order and final costs/first hops.
8. An LSP from R has sequence 10 in the database. Copies arrive with sequences 9, 10, and 11. Which replaces it and is re-flooded? Why is a lifetime still needed?
9. A 1300-byte IPv4 datagram with a 20-byte header and ID 417 crosses MTU 580. Give every fragment's payload length, total length, offset, MF, and DF assuming fragmentation is allowed. This is the 2015 exam's fragmentation pattern.
10. A fragment has offset 100, payload 512 bytes, and MF=1. Re-fragment it over MTU 276 with 20-byte headers. Give child offsets and MF values.
11. For host `10.104.216.101/20`, find mask, network, broadcast, first/last usable host, and number of usable hosts. This uses deck 6's broadcast example.
12. Split `200.23.16.0/24` into four equal subnets. List their prefixes and usable-host count per subnet. Then determine the smallest ordinary subnet that can hold 62 interface addresses.
13. Can `203.0.113.0/25` and `203.0.113.128/25` be summarized exactly? Can `203.0.113.64/26` and `203.0.113.128/26` be summarized as one exact prefix?
14. A table contains `0.0.0.0/0 -> A`, `201.10.0.0/21 -> B`, `201.10.6.0/23 -> C`, and `201.10.6.128/25 -> D`. Choose outputs for `201.10.6.17`, `201.10.6.200`, `201.10.7.4`, and `201.10.8.1`.
15. Name the DORA messages in order. Explain how an addressless client contacts a remote DHCP server, and identify who allocates the address.

## 12. Practice answers

### 1-4. Foundations and performance

1. No borrowing of the reserved 2 Mbps in the basic model. The other 8 Mbps can support other circuits; reserving one circuit does not reserve the whole link.
2. Transmission is $8000/10^7=0.8$ ms. Propagation is $200,000/(2\times10^8)=1$ ms. Doubling rate makes transmission 0.4 ms; propagation stays 1 ms.
3. $\min(12,9,20/4)=5$ Mbps. Ideal transfer time is $40/5=8$ seconds.
4. Application, transport, network, link, physical. OSI adds session and presentation. A typical forwarding router uses physical, link, and network layers; the transport/application endpoints remain at hosts.

### 5-8. Routing

5. Via A costs $3+8=11$; via B costs $6+2=8$. Select `<Z, 8, B>`. After B's change, compare 11 with $6+20=26$ and select `<Z, 11, A>`.
6. Y uses Z's stale cost 5 and adds the Y-Z cost 1. It cannot see that Z's old route depended on Y. With poisoned reverse, Z advertises X at infinity to Y while Z uses Y as next hop. This prevents that mutual dependency from being offered as a valid alternative, but longer loops remain possible.
7. Confirm A=0, B=2, C=3, D=4. After confirming B, improve C from 5 to 3 and discover D at 6. After confirming C, improve D to 4. All three remote destinations have first hop B.
8. Sequence 9 is older; 10 is a duplicate; 11 replaces the stored copy and is re-flooded. Lifetime removes stale information if R fails and no newer version arrives.

### 9-10. Fragmentation

For Question 9, payload is 1280 and capacity is $8\lfloor560/8\rfloor=560$:

| Fragment | ID | Payload | Total length | Offset | MF | DF |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| 1 | 417 | 560 | 580 | 0 | 1 | 0 |
| 2 | 417 | 560 | 580 | 70 | 1 | 0 |
| 3 | 417 | 160 | 180 | 140 | 0 | 0 |

For Question 10, each child carries 256 bytes. Offsets are **100 and 132**. Both have **MF=1** because the original fragment was not the end of the original datagram. Both total lengths are 276.

### 11-15. Addressing and DHCP

11. /20 mask is **255.255.240.0**. Third-octet block size is 16; 216 lies in the 208-223 block. Network **10.104.208.0**, broadcast **10.104.223.255**, usable range **10.104.208.1-10.104.223.254**, usable count $2^{12}-2=4094$.
12. `/26` subnets start at fourth octets **0, 64, 128, 192**, each with 62 usable addresses. A /26 is also the smallest ordinary subnet supporting 62 interface addresses; if 62 hosts plus an additional gateway are required, use a larger block.
13. The two /25s summarize to **203.0.113.0/24**. The two /26s are adjacent but cross a /25 boundary, so no single prefix covers exactly their union. Keep both; their smallest covering prefix is /24 and includes extra space.
14. Outputs are **C, D, C, A** respectively. Select the longest matching prefix separately for each address.
15. Discover, Offer, Request, Acknowledge. The client broadcasts locally; the relay unicasts to the server and forwards replies back. The **server** allocates the lease from the appropriate pool.

## 13. Using the old exams selectively

| Supplied paper | Useful questions within this guide | Material not justified by the current cutoff |
| --- | --- | --- |
| f15-0.pdf, printed Fall 2015 | Q1 Internet/poisoned reverse; Q2 DV state, LSP sequence numbers, Dijkstra; Q3 masks/subnets/fragmentation/reassembly; Q4(d-e) aggregation arithmetic | Certificate questions, BGP attributes/policy, and implementation question unless separately confirmed |
| s17-0.pdf, printed Fall 2017 | Q1 reliable flooding; Q2 LS/DV, LSP lifetime, poisoned reverse, periodic updates; Q3(a-b) subnetting/re-fragmentation; Q4(b) prefix-coverage reasoning | Anycast, ICMP echo/traceroute, BGP, Mobile IP, and implementation questions unless separately confirmed |
| s21-0.pdf, printed Fall 2021 | Q1 CIDR/poisoned reverse; Q2 routing comparisons/advertisements/TTL; Q3 subnetting and fragment sanity checks; Q4(b) aggregation | SACK, detailed AS/BGP policy, Mobile IP, and implementation question unless separately confirmed |

**Do not copy old-paper quirks into your rules.** The 2021 Q3(a) prefix needs normalization from `44.100.101.0/23` to `44.100.100.0/23`. Its Q3(b) states payload 532 with MF=1, although a non-final payload must be divisible by 8. The guide uses consistent examples and treats such input validation separately.

**Socket-programming scope check:** Your note and all three old papers include socket/implementation material, but the supplied E1 review and decks 1-6 do not list it. Keep it separate from the main preparation unless your instructor confirms assignment/API questions are included. If confirmed, review server `socket/bind/listen/accept`, client `socket/connect`, TCP partial `send/recv`, EOF/error handling, and why `select/poll` readiness does not promise an entire application message is available. Do not let those historical questions expand the stated theory scope on their own.

## 14. Final readiness checklist

- [ ] I can explain why circuit capacity can be idle without claiming the whole physical link is blocked.
- [ ] I distinguish forwarding, routing, bandwidth, throughput, transmission delay, and propagation delay.
- [ ] I can calculate delays and bottleneck throughput with correct units.
- [ ] I can name the five Internet layers and the two additional OSI layers.
- [ ] I can compute a DV update using each neighbor's advertised cost plus my direct-link cost.
- [ ] I can explain periodic updates, triggered updates, and the value of retaining neighbor vectors.
- [ ] I can trace the X-Y-Z cost increase and distinguish path estimates from physical link costs.
- [ ] I can apply split horizon and poisoned reverse to the correct neighbor and state their limits.
- [ ] I can describe LSP fields, version checking, flooding, aging, and the difference from IPv4 TTL.
- [ ] I can trace Dijkstra, including tentative/confirmed nodes, relaxation, and first-hop selection.
- [ ] I know RIP versus OSPF and can compare LS versus DV without absolute claims.
- [ ] I can interpret IPv4 IHL, total length, ID, flags, offset, TTL, protocol, and checksum.
- [ ] I can produce fragmentation tables, including re-fragmentation of a nonzero-offset fragment.
- [ ] I can explain reassembly completion, timeout, and malformed-fragment bounds.
- [ ] I can identify historical classes A-E and explain their masks, capacities, and limitations.
- [ ] I can derive a subnet's mask, network, broadcast, usable range, and host count.
- [ ] I can allocate equal and unequal aligned subnets without overlaps.
- [ ] I can aggregate prefixes without silently including unallocated space and apply longest-prefix matching.
- [ ] I can explain DORA, leases, configuration fields, and the purpose of DHCP relay.

## Sources and navigation

- Main course note: [[CMSC417]]. This guide preserves the broader note without removing later topics.
- Review: [417 E1 Review.pdf](<C:/Users/NYCXA/Downloads/417 E1 Review.pdf>), substantive handwritten pages 2-4. Its topic list drives the coverage emphasis here, not the old exams.
- [Deck 1: Foundations](<C:/Users/NYCXA/Downloads/1_foundations.pptx>): Internet, switching, edge/core, structure, queueing.
- [Deck 2: Internetworking and DV](<C:/Users/NYCXA/Downloads/2_internetworking_DV_routing.pptx>): delay/throughput, Internet/OSI layers, encapsulation, routing graphs, DV introduction.
- [Deck 3: DV and LS routing](<C:/Users/NYCXA/Downloads/3_internetworking_DV_LS_routing.pptx>): DV rounds, Bellman-Ford, advertisements, failures, count to infinity, mitigations.
- [Deck 4: LS routing and IP](<C:/Users/NYCXA/Downloads/4_internetworking_LSRouting_IP.pptx>): stale-route examples, RIP, LSP structure, LS strategy.
- [Deck 5: IP](<C:/Users/NYCXA/Downloads/5_internetworking_IP.pptx>): flooding, Dijkstra, OSPF, LS/DV comparison, IPv4 introduction/header.
- [Deck 6: Subnets, CIDR, DHCP](<C:/Users/NYCXA/Downloads/6_internetworking_subnet_CIDR_DHCP.pptx>): fragmentation/reassembly, classful addressing, subnetting, aggregation, longest-prefix matching, DHCP. Repeated/backup examples are consolidated.
- [Deck 7: DHCP before NAT](<C:/Users/NYCXA/Downloads/7_DHCP_NAT_ARP_ICMP_VPN_IPV6.pptx>): slides 2-15 only for this guide's main scope.
- Older practice papers: [2015](<C:/Users/NYCXA/Downloads/417-exams/f15-0.pdf>), [2017](<C:/Users/NYCXA/Downloads/417-exams/s17-0.pdf>), [2021](<C:/Users/NYCXA/Downloads/417-exams/s21-0.pdf>). Year labels above follow the papers' printed headers rather than assuming the filename's season label is accurate.
