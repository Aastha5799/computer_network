# LAB 3: SIMULATION OF NETWORK DEVICES USING CISCO PACKET TRACER

## Objective

1. To simulate different network devices using Cisco Packet Tracer.
2. To understand the function and working of basic networking devices.
---

## Software Required:
Cisco Packet Tracer
---

## Theory

### Cisco Packet Tracer

Cisco Packet Tracer is a powerful network simulation software used for learning and practicing computer networking concepts. It allows users to design network topologies, connect virtual devices, and configure them using a command-line interface similar to real Cisco devices. Since all networking components are simulated, users can visualize packet flow, test connectivity, and analyze network behavior without using physical hardware. This tool is widely used for academic learning and network experimentation.

### 1. Hub

A **Hub** is a basic networking device used to connect multiple computers in a network. It works at the Physical Layer of the OSI model. When a hub receives data from one device, it broadcasts the data to all other connected devices without checking the destination. Because of this, unnecessary traffic is created in the network and performance is reduced. Hubs are simple and inexpensive but are rarely used in modern networks.
1. Works at Data Link Layer (Layer 2)
2. Uses MAC address table
3. Reduces network traffic

---

### 2. Switch

A **Switch** is an intelligent networking device used in local area networks. It works at the Data Link Layer of the OSI model and uses MAC addresses to forward data. Unlike a hub, a switch sends data only to the destination device, which reduces network traffic and improves speed. Switches are widely used in modern networks due to their efficiency and better performance.
1. Works at Layer 2
2. Filters traffic using MAC address
3. Connects two LAN segments
---

### 3. Bridge

A **bridge** is used to divide a large network into smaller segments. It works at the Data Link Layer and filters data based on MAC addresses. By separating network segments, a bridge reduces traffic and improves overall network performance. It allows communication between different LAN segments while controlling unnecessary data flow.
1. Works at Layer 2
2. Filters traffic using MAC address
3. Connects two LAN segments

---

### 4. Router

A **Router** is a networking device that connects two or more different networks. It works at the Network Layer of the OSI model and routes data using IP addresses. Routers determine the best path for data using routing tables. They are commonly used to connect local networks to the internet and provide secure and efficient data transmission.
1. Works at Network Layer (Layer 3)
2. Uses routing table
3. Connects LAN to WAN

---

### 5. Repeater

A **Repeater** is a device used to extend the distance of a network. It works at the Physical Layer of the OSI model. When signals become weak over long distances, a repeater regenerates and strengthens them before forwarding. Repeaters help in maintaining signal quality but do not filter or manage data traffic.
1. Works at Physical Layer (Layer 1)
2. Boosts signal strength

---


## Observation

### 1. Hub Implementation

| Device Name | Interface | IP Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| PC0 | FastEthernet0 | 192.168.1.1 | 255.255.255.0 |
| PC1 | FastEthernet0 | 192.168.1.2 | 255.255.255.0 |
| PC2 | FastEthernet0 | 192.168.1.3 | 255.255.255.0 |
| PC3 | FastEthernet0 | 192.168.1.4 | 255.255.255.0 |
| PC4 | FastEthernet0 | 192.168.1.5 | 255.255.255.0 |
| PC5 | FastEthernet0 | 192.168.1.6 | 255.255.255.0 |

In this simulation, a star topology was created using a Hub. When a PDU was sent from PC0 to PC5, the hub broadcasted the packet to all connected devices. Only the destination device accepted the packet, while others ignored it.

---

### 2. Switch Implementation

| Device Name | Interface | IP Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| PC0 | FastEthernet0 | 192.168.1.10 | 255.255.255.0 |
| PC1 | FastEthernet0 | 192.168.1.11 | 255.255.255.0 |
| PC2 | FastEthernet0 | 192.168.1.12 | 255.255.255.0 |
| PC3 | FastEthernet0 | 192.168.1.13 | 255.255.255.0 |
| PC4 | FastEthernet0 | 192.168.1.14 | 255.255.255.0 |
| PC5 | FastEthernet0 | 192.168.1.15 | 255.255.255.0 |

The switch forwarded packets directly to the destination after learning MAC addresses, resulting in unicast transmission.

---

### 3. Bridge Implementation

| Device Name | IP Address | Subnet Mask |
| :--- | :--- | :--- |
| Switch0 | 192.168.1.2 | 255.255.255.0 |
| Switch1 | 10.10.10.2 | 255.255.255.0 |
| Bridge0 | 192.168.1.1 | 255.255.255.0 |

The bridge selectively forwarded traffic between network segments based on MAC address filtering.

---

### 4. Repeater Implementation

| Device Name | Connection Type | Status |
| :--- | :--- | :--- |
| Repeater0 | Port 0 to Left Segment | Active |
| Repeater0 | Port 1 to Right Segment | Active |

The repeater successfully regenerated signals to extend communication distance.

---

### 5. Router Implementation

| Device Name | Interface | IP Address | Subnet Mask | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| Router0 | Gig0/0 (LAN 1) | 192.168.1.1 | 255.255.255.0 | N/A |
| Router0 | Gig0/1 (LAN 2) | 10.10.10.1 | 255.255.255.0 | N/A |
| PC-LAN1 | FastEthernet0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC-LAN2 | FastEthernet0 | 10.10.10.2 | 255.255.255.0 | 10.10.10.1 |

The router enabled communication between two different LANs and functioned as a gateway.

---

## Discussion and Conclusion

In this lab, various networking devices were implemented and analyzed using Cisco Packet Tracer. The behavior of broadcast and unicast communication was clearly observed through PDU simulation. This practical session helped in understanding the roles and working principles of hubs, switches, bridges, routers, and repeaters. The lab was successfully completed, enhancing practical knowledge of computer networking concepts.

