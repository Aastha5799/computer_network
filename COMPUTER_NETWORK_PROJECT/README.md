# OSPF Network Configuration with VLANs and DHCP using Cisco Packet Tracer

---

## Objectives

- To configure OSPF dynamic routing between two routers connected via a serial link.
- To implement VLAN segmentation (VLAN 10 and VLAN 20) on switches connected to each router.
- To set up a centralized DHCP server on Router 1 to automatically assign IP addresses to all devices.
- To verify full network connectivity using ping and tracert commands between all devices.

---

## Software Required

- Cisco Packet Tracer

---

## Theory

### OSPF (Open Shortest Path First)
OSPF is a dynamic routing protocol that works at the Network Layer (Layer 3). Routers exchange Link State Advertisements (LSAs) to build a network map and use Dijkstra's algorithm to find the best path. Both routers must reach FULL adjacency state before routes are exchanged.

### VLAN (Virtual Local Area Network)
A VLAN logically separates network devices on the same physical switch into different broadcast domains. Trunk ports carry multiple VLANs using 802.1Q tagging while access ports carry traffic for a single VLAN.

### Router-on-a-Stick
A technique where one physical router interface is divided into subinterfaces, each handling a different VLAN. Each subinterface gets its own IP address and uses `encapsulation dot1Q` to tag frames.

### DHCP (Dynamic Host Configuration Protocol)
DHCP automatically assigns IP addresses to devices. Excluded addresses reserve IPs for routers. The `ip helper-address` command relays DHCP requests from remote subnets to the DHCP server.

### Serial WAN Link
A point-to-point connection between two routers. The DCE side sets the clock rate. A /30 subnet is used since only 2 usable IPs are needed.

---

## Network Topology

```
        [Router1]----Serial0/1/0----[Router2]
        172.16.0.1                  172.16.0.2
             |                           |
         Gi0/1 (trunk)              Gi0/1 (trunk)
             |                           |
         [Switch0]                   [Switch1]
          Fa0/3                       Fa0/3
         /      \                    /      \
      Fa0/1    Fa0/2             Fa0/1    Fa0/2
        |        |                 |        |
      [PC0]    [PC1]            [PC2]    [PC3]
    VLAN10   VLAN20           VLAN10   VLAN20
  192.168.10.1 192.168.20.1  10.10.10.1 10.10.20.1
```

---

## Configuration Table

### Router Interface Configuration

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| Router1 (R1) | Serial0/1/0 | 172.16.0.1 | 255.255.255.252 | N/A |
| Router1 (R1) | GigabitEthernet0/1.10 | 192.168.10.1 | 255.255.255.0 | N/A |
| Router1 (R1) | GigabitEthernet0/1.20 | 192.168.20.1 | 255.255.255.0 | N/A |
| Router2 (R2) | Serial0/1/0 | 172.16.0.2 | 255.255.255.252 | N/A |
| Router2 (R2) | GigabitEthernet0/1.10 | 10.10.10.1 | 255.255.255.0 | N/A |
| Router2 (R2) | GigabitEthernet0/1.20 | 10.10.20.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | 192.168.10.11 (DHCP) | 255.255.255.0 | 192.168.10.1 |
| PC1 | FastEthernet0 | 192.168.20.11 (DHCP) | 255.255.255.0 | 192.168.20.1 |
| PC2 | FastEthernet0 | 10.10.10.11 (DHCP) | 255.255.255.0 | 10.10.10.1 |
| PC3 | FastEthernet0 | 10.10.20.11 (DHCP) | 255.255.255.0 | 10.10.20.1 |

### Switch Port Configuration

| Device | Interface | Mode | VLAN |
|---|---|---|---|
| Switch0 | FastEthernet0/1 | Access | VLAN 10 |
| Switch0 | FastEthernet0/2 | Access | VLAN 20 |
| Switch0 | FastEthernet0/3 | Trunk | 1, 10, 20 |
| Switch1 | FastEthernet0/1 | Access | VLAN 10 |
| Switch1 | FastEthernet0/2 | Access | VLAN 20 |
| Switch1 | FastEthernet0/3 | Trunk | 1, 10, 20 |

### DHCP Pool Configuration (Router 1)

| Pool Name | Network | Default Gateway | DNS Server |
|---|---|---|---|
| VLAN10_R1 | 192.168.10.0/24 | 192.168.10.1 | 8.8.8.8 |
| VLAN20_R1 | 192.168.20.0/24 | 192.168.20.1 | 8.8.8.8 |
| VLAN10_R2 | 10.10.10.0/24 | 10.10.10.1 | 8.8.8.8 |
| VLAN20_R2 | 10.10.20.0/24 | 10.10.20.1 | 8.8.8.8 |

### OSPF Configuration

| Device | Router-ID | Area |
|---|---|---|
| Router1 | 1.1.1.1 | Area 0 |
| Router2 | 2.2.2.2 | Area 0 |

---

## Process

### A. Build the Topology
1. Open Cisco Packet Tracer and create a new project
2. Add 2x Router 1941, 2x Switch 2960-24TT, 4x PC-PT
3. Install HWIC-2T module on both routers (Physical tab > Power OFF > drag module > Power ON)
4. Connect PCs to switches and switches to routers using Copper Straight-Through cable
5. Connect routers using Serial DCE cable on Serial0/1/0 ports

### B. Configure Switch0 and Switch1
1. Create VLAN 10 and VLAN 20
2. Set Fa0/3 as trunk port allowing VLANs 1, 10, 20
3. Set Fa0/1 as access port for VLAN 10
4. Set Fa0/2 as access port for VLAN 20

### C. Configure Router 1
1. Assign IP 172.16.0.1/30 to Serial0/1/0 with clock rate 64000
2. Create subinterfaces Gi0/1.10 and Gi0/1.20 with dot1Q encapsulation
3. Configure 4 DHCP pools for all subnets
4. Enable OSPF process 1 with router-id 1.1.1.1 in Area 0

### D. Configure Router 2
1. Assign IP 172.16.0.2/30 to Serial0/1/0 (no clock rate)
2. Create subinterfaces Gi0/1.10 and Gi0/1.20 with dot1Q encapsulation
3. Add ip helper-address 172.16.0.1 on both subinterfaces
4. Enable OSPF process 1 with router-id 2.2.2.2 in Area 0

### E. Test Connectivity
1. Set each PC to DHCP via Desktop > IP Configuration > DHCP
2. Verify IP assignment using ipconfig
3. Test using ping and tracert commands

---

## Configuration Commands

### Switch0 and Switch1
```
enable
configure terminal
hostname SW0
vlan 10
 name VLAN10
vlan 20
 name VLAN20
exit
interface FastEthernet0/3
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
end
write memory
```

### Router 1
```
enable
configure terminal
hostname R1
interface Serial0/1/0
 ip address 172.16.0.1 255.255.255.252
 clock rate 64000
 no shutdown
interface GigabitEthernet0/1
 no ip address
 no shutdown
interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp excluded-address 10.10.20.1 10.10.20.10
ip dhcp pool VLAN10_R1
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20_R1
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
ip dhcp pool VLAN10_R2
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20_R2
 network 10.10.20.0 255.255.255.0
 default-router 10.10.20.1
 dns-server 8.8.8.8
router ospf 1
 router-id 1.1.1.1
 network 172.16.0.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/1.10
 passive-interface GigabitEthernet0/1.20
end
write memory
```

### Router 2
```
enable
configure terminal
hostname R2
interface Serial0/1/0
 ip address 172.16.0.2 255.255.255.252
 no shutdown
interface GigabitEthernet0/1
 no ip address
 no shutdown
interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.10.10.1 255.255.255.0
 ip helper-address 172.16.0.1
 no shutdown
interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.10.20.1 255.255.255.0
 ip helper-address 172.16.0.1
 no shutdown
router ospf 1
 router-id 2.2.2.2
 network 172.16.0.0 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.20.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/1.10
 passive-interface GigabitEthernet0/1.20
end
write memory
```

---

## Result

OSPF adjacency reached FULL state between both routers. All 4 PCs received IPs via DHCP automatically. Ping tests showed 0% packet loss and tracert confirmed 2-hop routing across the serial link.

| Test | Result |
|---|---|
| OSPF Adjacency | FULL |
| PC0 DHCP | 192.168.10.11 |
| PC1 DHCP | 192.168.20.11 |
| PC2 DHCP | 10.10.10.11 |
| PC3 DHCP | 10.10.20.11 |
| Ping cross-router | 0% loss |
| Tracert | 2 hops |

---

## Discussion and Conclusion

In this lab, a network was built using two routers, two switches, and four PCs in Cisco Packet Tracer. VLAN 10 and VLAN 20 were created on both switches using the Router-on-a-Stick technique, where subinterfaces on each router handled inter-VLAN routing. OSPF was configured on both routers in Area 0 and the adjacency successfully reached FULL state.

The DHCP server on Router 1 assigned IP addresses to all four PCs. The `ip helper-address` command on Router 2 forwarded DHCP requests from PC2 and PC3 to Router 1. Connectivity was verified using ping and tracert commands which confirmed 0% packet loss and correct 2-hop routing.

Overall, the lab was completed successfully. All objectives were achieved and the network operated with full end-to-end connectivity between all devices.

---

