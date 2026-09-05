## Foundations and Internetworking

### Internet basics
**Internet:** A network of networks: end systems connect through access Internet Service Provider (ISPs), which interconnect through regional networks, large tier-1 ISPs, and Internet exchange points (IXPs).

- **Hosts / end systems:** Devices running network applications, such as clients and servers.
- **Communication links:** Fiber, copper, radio, or satellite; their transmission rate is the **bandwidth**.
- **Packet switches:** Routers and switches that forward chunks of data called packets.
- **Protocol:** Defines message formats, message order, and the actions taken when messages are sent or received. Internet standards are commonly published as RFCs by the IETF.
- The Internet also provides applications with a programming interface for sending and receiving data.

### Circuit switching vs. packet switching
**Circuit switching:** Reserves end-to-end resources for a call. It provides predictable performance, but reserved capacity sits idle when the sender has no data.

**Packet switching:** Splits data into packets that share links. Routers queue packets when the output link is busy; a full buffer drops arriving packets.

**Multiplexing:** Multiple logical flows share one physical link.

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
