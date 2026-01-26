# LAB 4: Subnetting and Supernetting Using Cisco Packet Tracer

## Objectives
1. To study how IP networks are logically segmented using subnetting and how multiple networks can be combined using supernetting.

2. To plan and apply fixed-length or variable-length subnet mask (FLSM/VLSM) addressing schemes and implement the network topology in Cisco Packet Tracer in order to observe packet transmission between different subnets through a router.

## Software and Hardware Requirements
1. Cisco Packet Tracer 
2. Windows PC/Laptop

## Theory

### 1. Subnetting
Subnetting is the process of breaking one large physical network into several smaller
logical networks called subnets. This is done by borrowing bits from the host part of an IP
address to create additional network identifiers.

*   **Operation:** A subnet mask is used to extend the network portion of the IP address. For
example, borrowing 2 bits from a Class C (/24) network results in 4 smaller subnets.
Routers use the subnet mask to identify which subnet an IP address belongs to and route
traffic correctly.
*   **Application:** Subnetting improves network performance, management, and security by
reducing broadcast traffic. It keeps local traffic within the same subnet and prevents
unnecessary data from spreading across the entire network, making the network more
efficient and organized.

### 2. Supernetting
Supernetting, also known as Classless Inter-Domain Routing (CIDR) or route
summarization, is the reverse of subnetting. It allows multiple smaller, consecutive
networks to be merged into a single larger network

*   **Operation:** Supernetting is done by reducing the number of network bits in the subnet
mask, effectively shifting the network boundary to the left. This creates one summary
(aggregate) address that represents a group of contiguous networks instead of listing each
one separately.
*   **Application:** Supernetting is mainly used to reduce the size of routing tables in routers.
Smaller routing tables require less memory and processing power, which speeds up routing
decisions and improves overall Internet efficiency and scalability

---

## Network Design

### Subnetting Calculations
*   **Base network:** 192.168.1.0/24
*   **Required number of subnets:** 4
*   **Number of IP addresses per subnet:** 64 (Block size)
*    /26 (Borrowed 2 bits: $2^2 = 4$ subnets)
*   **Subnet Mask:** 255.255.255.192

### Subnet Calculation Table

| Subnet | Network ID | 1st Host | Last Host | Broadcast ID |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 192.168.1.0 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| 2 | 192.168.1.64 | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 |
| 3 | 192.168.1.128 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 |
| 4 | 192.168.1.192 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 |

### Network Topology

**Subnetting:**
A topology was created using Router0 to connect two subnets: 192.168.1.0/26 and 192.168.1.64/26. Switch0 connects PC0, PC1, and PC2 in the first subnet, while Switch1 connects PC3, PC4, and PC5 in the second subnet. Both switches are connected to the router interfaces.

**Supernetting:**
The topology consists of one router, one switch, and four PCs (PC6–PC9). All PCs connect to the switch, which is connected to the router.

---

## Configuration Tables

### Subnetting Configuration

| Device | IPv4 | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- |
| Router0 (FastEthernet0/0) | 192.168.1.1 | 255.255.255.192 | N/A |
| Router0 (FastEthernet0/1) | 192.168.1.65 | 255.255.255.192 | N/A |
| PC0 (Subnet 1) | 192.168.1.2 | 255.255.255.192 | 192.168.1.1 |
| PC1 (Subnet 1) | 192.168.1.3 | 255.255.255.192 | 192.168.1.1 |
| PC2 (Subnet 1) | 192.168.1.4 | 255.255.255.192 | 192.168.1.1 |
| PC3 (Subnet 2) | 192.168.1.66 | 255.255.255.192 | 192.168.1.65 |
| PC4 (Subnet 2) | 192.168.1.67 | 255.255.255.192 | 192.168.1.65 |
| PC5 (Subnet 2) | 192.168.1.68 | 255.255.255.192 | 192.168.1.65 |

### Supernetting Configuration

| Device | IPv4 | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- |
| Router0 (FastEthernet0/0) | 192.168.0.1 | 255.255.252.0 | N/A |
| PC0 | 192.168.0.10 | 255.255.252.0 | 192.168.0.1 |
| PC1 | 192.168.1.10 | 255.255.252.0 | 192.168.0.1 |
| PC2 | 192.168.2.10 | 255.255.252.0 | 192.168.0.1 |
| PC3 | 192.168.3.10 | 255.255.252.0 | 192.168.0.1 |

---

## Connectivity Testing

*   **Ping PC0 to PC2:** Successful
*   **Ping PC0 to PC5:** Successful
*   **Ping PC0 to PC3:** Successful

## Result
The network was successfully configured and tested for connectivity. Ping requests from
PC0 (192.168.1.2) to PC2 (192.168.1.4) within Subnet 1 and from PC0 to PC4
(192.168.1.67) in Subnet 2 were completed successfully. Similar ping tests were also
performed for the supernetting configuration, and all packets were transmitted and
received correctly, confirming that the network connections were properly set up.

## Discussion and Conclusion
During the lab session, we implemented the concepts of subnetting and supernetting to
gain a better understanding of IP address management and efficient network design.
Subnetting allowed us to divide a large network into multiple smaller subnets, which
helped in better utilization of IP addresses and reduced unnecessary network traffic. It also
improved network performance by limiting the broadcast domain and making the network
easier to manage.
On the other hand, supernetting enabled the combination of multiple smaller networks into
a single larger network. This simplified routing by reducing the number of routing entries
required in routing tables. Through these practical exercises, we learned how subnetting
and supernetting play a crucial role in optimizing network performance, improving
scalability, and ensuring effective communication in modern networks.
Hence, the lab was successfully completed with proper understanding and practical
implementation of subnetting and supernetting using Cisco Packet Tracer.