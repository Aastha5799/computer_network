# OSPF Network Project — Cisco Packet Tracer

This project builds a simple network with 2 routers, 2 switches, and 4 PCs using Cisco Packet Tracer.

---

## What This Project Does

- Two routers talk to each other using OSPF (automatic routing)
- Each router has a switch with 2 VLANs (VLAN 10 and VLAN 20)
- Router 1 gives IP addresses to all PCs automatically (DHCP)
- All 4 PCs can ping each other

---

## Network Layout

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
  192.168.10.x 192.168.20.x  10.10.10.x 10.10.20.x
```

| Device | IP Address |
|---|---|
| Router1 Serial | 172.16.0.1 |
| Router2 Serial | 172.16.0.2 |
| Router1 VLAN10 | 192.168.10.1 |
| Router1 VLAN20 | 192.168.20.1 |
| Router2 VLAN10 | 10.10.10.1 |
| Router2 VLAN20 | 10.10.20.1 |
| PC0 | 192.168.10.11 (auto) |
| PC1 | 192.168.20.11 (auto) |
| PC2 | 10.10.10.11 (auto) |
| PC3 | 10.10.20.11 (auto) |

---

## Devices Needed

- 2 Routers (Cisco 1941) — add HWIC-2T module for serial port
- 2 Switches (Cisco 2960)
- 4 PCs

---

## Step 1 — Connect the Devices

- PC0 → Switch0 (Fa0/1)
- PC1 → Switch0 (Fa0/2)
- Switch0 → Router1 (Fa0/3 to Gi0/1)
- PC2 → Switch1 (Fa0/1)
- PC3 → Switch1 (Fa0/2)
- Switch1 → Router2 (Fa0/3 to Gi0/1)
- Router1 → Router2 using Serial DCE cable (Serial0/1/0)

---

## Step 2 — Configure Switch0

```
enable
configure terminal
hostname SW0
vlan 10
vlan 20
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

---

## Step 3 — Configure Switch1

```
enable
configure terminal
hostname SW1
vlan 10
vlan 20
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

---

## Step 4 — Configure Router1

```
enable
configure terminal
hostname R1

interface Serial0/1/0
 ip address 172.16.0.1 255.255.255.252
 clock rate 64000
 no shutdown
exit

interface GigabitEthernet0/1
 no ip address
 no shutdown
exit

interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp excluded-address 10.10.20.1 10.10.20.10

ip dhcp pool VLAN10_R1
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
exit

ip dhcp pool VLAN20_R1
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
exit

ip dhcp pool VLAN10_R2
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.1
 dns-server 8.8.8.8
exit

ip dhcp pool VLAN20_R2
 network 10.10.20.0 255.255.255.0
 default-router 10.10.20.1
 dns-server 8.8.8.8
exit

router ospf 1
 router-id 1.1.1.1
 network 172.16.0.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/1.10
 passive-interface GigabitEthernet0/1.20
exit

end
write memory
```

---

## Step 5 — Configure Router2

```
enable
configure terminal
hostname R2

interface Serial0/1/0
 ip address 172.16.0.2 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/1
 no ip address
 no shutdown
exit

interface GigabitEthernet0/1.10
 encapsulation dot1Q 10
 ip address 10.10.10.1 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1.20
 encapsulation dot1Q 20
 ip address 10.10.20.1 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1.10
 ip helper-address 172.16.0.1
exit

interface GigabitEthernet0/1.20
 ip helper-address 172.16.0.1
exit

router ospf 1
 router-id 2.2.2.2
 network 172.16.0.0 0.0.0.3 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.20.0 0.0.0.255 area 0
 passive-interface GigabitEthernet0/1.10
 passive-interface GigabitEthernet0/1.20
exit

end
write memory
```

---

## Step 6 — Set PCs to DHCP

For each PC click: **Desktop → IP Configuration → DHCP**

---

## Step 7 — Test the Network

On Router1 CLI:
```
show ip ospf neighbor
show ip dhcp binding
ping 172.16.0.2
```

On PC0 Command Prompt:
```
ping 192.168.10.1
ping 10.10.10.1
tracert 10.10.20.1
```

---

## Expected Results

- OSPF neighbor state = FULL
- All 4 PCs get IP automatically
- Ping between all devices = 0% loss
- Tracert shows 2 hops between routers

---

## What I Learned

- **VLAN** — separates network traffic between departments
- **Trunk port** — carries multiple VLANs between switch and router
- **Router-on-a-Stick** — one router interface handles multiple VLANs using subinterfaces
- **OSPF** — routers automatically share routes with each other
- **DHCP** — automatically gives IP addresses to PCs
- **DHCP Relay** — forwards DHCP requests from Router2 side to Router1
- **Serial link** — WAN connection between two routers