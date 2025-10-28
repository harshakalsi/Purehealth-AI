# dc1-leaf2c

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
  - [Port-Channel Interfaces](#port-channel-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | 10.118.5.19/24 | 10.118.5.254 |

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
   ip address 10.118.5.19/24
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

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
spanning-tree mst 0 priority 32768
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
| Ethernet1 | L2_KDC-AR7060X6-PCS-BELEAF1_Ethernet8 | *trunk | *none | *- | *- | 1 |
| Ethernet2 | L2_KDC-AR7060X6-PCS-BELEAF2_Ethernet8 | *trunk | *none | *- | *- | 1 |
| Ethernet5 | SERVER_dc1-leaf2-server1_iLO | access | 11 | - | - | - |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description L2_KDC-AR7060X6-PCS-BELEAF1_Ethernet8
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description L2_KDC-AR7060X6-PCS-BELEAF2_Ethernet8
   no shutdown
   channel-group 1 mode active
!
interface Ethernet5
   description SERVER_dc1-leaf2-server1_iLO
   no shutdown
   switchport access vlan 11
   switchport mode access
   switchport
   spanning-tree portfast
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | L2_BE_LEAF_Port-Channel8 | trunk | none | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description L2_BE_LEAF_Port-Channel8
   no shutdown
   switchport trunk allowed vlan none
   switchport mode trunk
   switchport
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
| default | False |
| PCS-NETINFRA-OOB | False |

#### IP Routing Device Configuration

```eos
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

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
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
