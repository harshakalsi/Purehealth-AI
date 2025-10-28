# dc1-spine2

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [Domain Lookup](#domain-lookup)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [TACACS Servers](#tacacs-servers)
  - [IP TACACS Source Interfaces](#ip-tacacs-source-interfaces)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
  - [AAA Accounting](#aaa-accounting)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | 10.118.5.12/24 | 10.118.5.254 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   vrf PCS-NETINFRA-OOB
   ip address 10.118.5.12/24
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 10.113.200.12 | PCS-NETINFRA-OOB | - |
| 10.118.200.12 | PCS-NETINFRA-OOB | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf PCS-NETINFRA-OOB 10.113.200.12
ip name-server vrf PCS-NETINFRA-OOB 10.118.200.12
```

### Domain Lookup

#### DNS Domain Lookup Summary

| Source interface | vrf |
| ---------------- | --- |
| Management1 | PCS-NETINFRA-OOB |

#### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf PCS-NETINFRA-OOB source-interface Management1
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 10.113.200.3 | PCS-NETINFRA-OOB | True | - | - | - | - | - | - | - |
| 10.118.200.3 | PCS-NETINFRA-OOB | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server vrf PCS-NETINFRA-OOB 10.113.200.3 prefer
ntp server vrf PCS-NETINFRA-OOB 10.118.200.3
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| PCS-NETINFRA-OOB | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf PCS-NETINFRA-OOB
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| arista | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username arista privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### TACACS Servers

#### TACACS Servers

| VRF | TACACS Servers | Single-Connection | Timeout |
| --- | -------------- | ----------------- | ------- |
| PCS-NETINFRA-OOB | 10.101.100.50 | False | - |
| PCS-NETINFRA-OOB | 10.100.100.50 | False | - |

#### TACACS Servers Device Configuration

```eos
!
tacacs-server host 10.101.100.50 vrf PCS-NETINFRA-OOB key 7 <removed>
tacacs-server host 10.100.100.50 vrf PCS-NETINFRA-OOB key 7 <removed>
```

### IP TACACS Source Interfaces

#### IP TACACS Source Interfaces

| VRF | Source Interface Name |
| --- | --------------- |
| PCS-NETINFRA-OOB | Management1 |

#### IP TACACS Source Interfaces Device Configuration

```eos
!
ip tacacs vrf PCS-NETINFRA-OOB source-interface Management1
```

### AAA Server Groups

#### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| PCSAUTHGRP | tacacs+ | PCS-NETINFRA-OOB | 10.101.100.50 |
| PCSAUTHGRP | tacacs+ | PCS-NETINFRA-OOB | 10.100.100.50 |

#### AAA Server Groups Device Configuration

```eos
!
aaa group server tacacs+ PCSAUTHGRP
   server 10.101.100.50 vrf PCS-NETINFRA-OOB
   server 10.100.100.50 vrf PCS-NETINFRA-OOB
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | group PCSAUTHGRP local |
| Login | console | local group PCSAUTHGRP |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default group PCSAUTHGRP local
aaa authentication login console local group PCSAUTHGRP
aaa authentication enable default group PCSAUTHGRP local
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | group PCSAUTHGRP local |

Authorization for configuration commands is disabled.

#### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | group PCSAUTHGRP local |

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default group PCSAUTHGRP local
aaa authorization commands all default group PCSAUTHGRP local
!
```

### AAA Accounting

#### AAA Accounting Summary

| Type | Commands | Record type | Groups | Logging |
| ---- | -------- | ----------- | ------ | ------- |
| Exec - Default | - | start-stop | PCSAUTHGRP local | False |
| Commands - Default | all | start-stop | PCSAUTHGRP | False |

#### AAA Accounting Device Configuration

```eos
aaa accounting exec default start-stop group PCSAUTHGRP local
aaa accounting commands all default start-stop group PCSAUTHGRP
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 10.113.5.1:9910 | PCS-NETINFRA-OOB | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | - | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.113.5.1:9910 -cvauth=token,/tmp/token -cvvrf=PCS-NETINFRA-OOB -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs -cvsourceintf=Management1
   no shutdown
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_KDC-AR7508R3-PCS-FELEAF1_Ethernet2 | - | 10.255.255.2/31 | default | 9214 | False | - | - |
| Ethernet2 | P2P_KDC-AR7508R3-PCS-FELEAF2_Ethernet2 | - | 10.255.255.6/31 | default | 9214 | False | - | - |
| Ethernet3 | P2P_KDC-AR7060X6-PCS-BELEAF1_Ethernet2 | - | 10.255.255.10/31 | default | 9214 | False | - | - |
| Ethernet4 | P2P_KDC-AR7060X6-PCS-BELEAF2_Ethernet2 | - | 10.255.255.14/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_KDC-AR7508R3-PCS-FELEAF1_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.255.2/31
!
interface Ethernet2
   description P2P_KDC-AR7508R3-PCS-FELEAF2_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.255.6/31
!
interface Ethernet3
   description P2P_KDC-AR7060X6-PCS-BELEAF1_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.255.10/31
!
interface Ethernet4
   description P2P_KDC-AR7060X6-PCS-BELEAF2_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 10.255.255.14/31
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.255.0.2/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.255.0.2/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| PCS-NETINFRA-OOB | False |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf PCS-NETINFRA-OOB
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| PCS-NETINFRA-OOB | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| PCS-NETINFRA-OOB | 0.0.0.0/0 | 10.118.5.254 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf PCS-NETINFRA-OOB 0.0.0.0/0 10.118.5.254
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65100 | 10.255.0.2 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.118.0.3 | 65371 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.118.0.4 | 65371 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.118.0.5 | 65372 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.118.0.6 | 65372 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.255.3 | 65371 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.255.7 | 65371 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.255.11 | 65372 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.255.15 | 65372 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Peer-tag In | Peer-tag Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ----------- | ------------ | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True | - | - | - | - | default | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65100
   router-id 10.255.0.2
   no bgp default ipv4-unicast
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.118.0.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.118.0.3 remote-as 65371
   neighbor 10.118.0.3 description KDC-AR7508R3-PCS-FELEAF1_Loopback0
   neighbor 10.118.0.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.118.0.4 remote-as 65371
   neighbor 10.118.0.4 description KDC-AR7508R3-PCS-FELEAF2_Loopback0
   neighbor 10.118.0.5 peer group EVPN-OVERLAY-PEERS
   neighbor 10.118.0.5 remote-as 65372
   neighbor 10.118.0.5 description KDC-AR7060X6-PCS-BELEAF1_Loopback0
   neighbor 10.118.0.6 peer group EVPN-OVERLAY-PEERS
   neighbor 10.118.0.6 remote-as 65372
   neighbor 10.118.0.6 description KDC-AR7060X6-PCS-BELEAF2_Loopback0
   neighbor 10.255.255.3 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.3 remote-as 65371
   neighbor 10.255.255.3 description KDC-AR7508R3-PCS-FELEAF1_Ethernet2
   neighbor 10.255.255.7 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.7 remote-as 65371
   neighbor 10.255.255.7 description KDC-AR7508R3-PCS-FELEAF2_Ethernet2
   neighbor 10.255.255.11 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.11 remote-as 65372
   neighbor 10.255.255.11 description KDC-AR7060X6-PCS-BELEAF1_Ethernet2
   neighbor 10.255.255.15 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.15 remote-as 65372
   neighbor 10.255.255.15 description KDC-AR7060X6-PCS-BELEAF2_Ethernet2
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.0.0/27 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.255.0.0/27 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| PCS-NETINFRA-OOB | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance PCS-NETINFRA-OOB
```
