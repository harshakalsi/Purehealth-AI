# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| FABRIC | l2leaf | dc1-leaf1c | 10.118.5.18/24 | vEOS-lab | Provisioned | - |
| FABRIC | l2leaf | dc1-leaf2c | 10.118.5.19/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine1 | 10.118.5.11/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine2 | 10.118.5.12/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | KDC-AR7060X6-PCS-BELEAF1 | 10.118.5.116/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | KDC-AR7060X6-PCS-BELEAF2 | 10.118.5.117/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | 10.118.5.16/24 | vEOS-lab | Provisioned | - |
| FABRIC | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | 10.118.5.17/24 | vEOS-lab | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l2leaf | dc1-leaf1c | Ethernet1 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet8 |
| l2leaf | dc1-leaf1c | Ethernet2 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet8 |
| l2leaf | dc1-leaf2c | Ethernet1 | l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet8 |
| l2leaf | dc1-leaf2c | Ethernet2 | l3leaf | KDC-AR7060X6-PCS-BELEAF2 | Ethernet8 |
| spine | dc1-spine1 | Ethernet1 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet1 |
| spine | dc1-spine1 | Ethernet2 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet1 |
| spine | dc1-spine1 | Ethernet3 | l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet1 |
| spine | dc1-spine1 | Ethernet4 | l3leaf | KDC-AR7060X6-PCS-BELEAF2 | Ethernet1 |
| spine | dc1-spine2 | Ethernet1 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet2 |
| spine | dc1-spine2 | Ethernet2 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet2 |
| spine | dc1-spine2 | Ethernet3 | l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet2 |
| spine | dc1-spine2 | Ethernet4 | l3leaf | KDC-AR7060X6-PCS-BELEAF2 | Ethernet2 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet3 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet3 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet4 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet4 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet3 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet3 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet4 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet4 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.255.255.0/26 | 64 | 16 | 25.0 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| dc1-spine1 | Ethernet1 | 10.255.255.0/31 | KDC-AR7508R3-PCS-FELEAF1 | Ethernet1 | 10.255.255.1/31 |
| dc1-spine1 | Ethernet2 | 10.255.255.4/31 | KDC-AR7508R3-PCS-FELEAF2 | Ethernet1 | 10.255.255.5/31 |
| dc1-spine1 | Ethernet3 | 10.255.255.8/31 | KDC-AR7060X6-PCS-BELEAF1 | Ethernet1 | 10.255.255.9/31 |
| dc1-spine1 | Ethernet4 | 10.255.255.12/31 | KDC-AR7060X6-PCS-BELEAF2 | Ethernet1 | 10.255.255.13/31 |
| dc1-spine2 | Ethernet1 | 10.255.255.2/31 | KDC-AR7508R3-PCS-FELEAF1 | Ethernet2 | 10.255.255.3/31 |
| dc1-spine2 | Ethernet2 | 10.255.255.6/31 | KDC-AR7508R3-PCS-FELEAF2 | Ethernet2 | 10.255.255.7/31 |
| dc1-spine2 | Ethernet3 | 10.255.255.10/31 | KDC-AR7060X6-PCS-BELEAF1 | Ethernet2 | 10.255.255.11/31 |
| dc1-spine2 | Ethernet4 | 10.255.255.14/31 | KDC-AR7060X6-PCS-BELEAF2 | Ethernet2 | 10.255.255.15/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.118.0.0/24 | 256 | 4 | 1.57 % |
| 10.255.0.0/27 | 32 | 2 | 6.25 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | dc1-spine1 | 10.255.0.1/32 |
| FABRIC | dc1-spine2 | 10.255.0.2/32 |
| FABRIC | KDC-AR7060X6-PCS-BELEAF1 | 10.118.0.5/32 |
| FABRIC | KDC-AR7060X6-PCS-BELEAF2 | 10.118.0.6/32 |
| FABRIC | KDC-AR7508R3-PCS-FELEAF1 | 10.118.0.3/32 |
| FABRIC | KDC-AR7508R3-PCS-FELEAF2 | 10.118.0.4/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 10.118.1.0/25 | 128 | 4 | 3.13 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | KDC-AR7060X6-PCS-BELEAF1 | 10.118.1.5/32 |
| FABRIC | KDC-AR7060X6-PCS-BELEAF2 | 10.118.1.5/32 |
| FABRIC | KDC-AR7508R3-PCS-FELEAF1 | 10.118.1.3/32 |
| FABRIC | KDC-AR7508R3-PCS-FELEAF2 | 10.118.1.3/32 |
