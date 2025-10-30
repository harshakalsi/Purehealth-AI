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
| FABRIC | spine | dc1-spine1 | 10.118.5.11/24 | vEOS-lab | Provisioned | - |
| FABRIC | spine | dc1-spine2 | 10.118.5.12/24 | vEOS-lab | Provisioned | - |
| FABRIC | l2leaf | KDC-AR7010TX-PCS-OOB1 | 10.118.5.41/24 | vEOS-lab | Provisioned | - |
| FABRIC | l2leaf | KDC-AR7010TX-PCS-OOB2 | 10.118.5.42/24 | vEOS-lab | Provisioned | - |
| FABRIC | l2leaf | KDC-AR7010TX-PCS-OOB3 | 10.118.5.43/24 | vEOS-lab | Provisioned | - |
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
| l2leaf | KDC-AR7010TX-PCS-OOB1 | Ethernet49 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet6/47/1 |
| l2leaf | KDC-AR7010TX-PCS-OOB1 | Ethernet50 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet6/47/1 |
| l2leaf | KDC-AR7010TX-PCS-OOB2 | Ethernet49 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet6/48/1 |
| l2leaf | KDC-AR7010TX-PCS-OOB2 | Ethernet50 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet6/48/1 |
| l2leaf | KDC-AR7010TX-PCS-OOB3 | Ethernet49 | l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet7/39/1 |
| l2leaf | KDC-AR7010TX-PCS-OOB3 | Ethernet50 | l3leaf | KDC-AR7508R3-PCS-FELEAF2 | Ethernet7/37/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet23/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet23/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet24/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet24/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet25/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet25/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet26/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet26/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet27/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet27/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet28/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet28/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet29/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet29/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet30/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet30/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet31/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet31/1 |
| l3leaf | KDC-AR7060X6-PCS-BELEAF1 | Ethernet32/1 | mlag_peer | KDC-AR7060X6-PCS-BELEAF2 | Ethernet32/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet3/21/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet3/21/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet3/22/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet3/22/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet3/23/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet3/23/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet3/24/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet3/24/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet6/49/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet6/49/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet6/50/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet6/50/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet7/49/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet7/49/1 |
| l3leaf | KDC-AR7508R3-PCS-FELEAF1 | Ethernet7/50/1 | mlag_peer | KDC-AR7508R3-PCS-FELEAF2 | Ethernet7/50/1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.255.255.0/26 | 64 | 0 | 0.0 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

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
