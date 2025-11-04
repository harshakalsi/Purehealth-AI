# KDC-AR7010TX-PCS-OOB2

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Domain Lookup](#domain-lookup)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management SSH](#management-ssh)
  - [Management Console](#management-console)
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
  - [Logging](#logging)
  - [SNMP](#snmp)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Queue Monitor](#queue-monitor)
  - [Queue Monitor Length](#queue-monitor-length)
  - [Queue Monitor Streaming](#queue-monitor-streaming)
  - [Queue Monitor Configuration](#queue-monitor-configuration)
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
| Management1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | 10.118.5.42/24 | 10.118.5.254 |

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
   ip address 10.118.5.42/24
```

### DNS Domain

DNS domain: purecloud.ae

#### DNS Domain Device Configuration

```eos
dns domain purecloud.ae
!
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

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **Asia/Dubai**.

#### Clock Device Configuration

```eos
!
clock timezone Asia/Dubai
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

### Management SSH

#### VRFs

| VRF | Enabled | IPv4 ACL | IPv6 ACL |
| --- | ------- | -------- | -------- |
| PCS-NETINFRA-OOB | True | - | - |
| default | False | - | - |

#### Other SSH Settings

| Idle Timeout | Connection Limit | Max from a single Host | Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
| ------------ | ---------------- | ---------------------- | ------- | -------------------- | -------------- | ------------------------- |
| 15 | - | - | default | default | default | default |

#### Management SSH Device Configuration

```eos
!
management ssh
   idle-timeout 15
   !
   vrf PCS-NETINFRA-OOB
      no shutdown
```

### Management Console

#### Management Console Timeout

Management Console Timeout is set to **20** minutes.

#### Management Console Device Configuration

```eos
!
management console
   idle-timeout 20
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
| Exec - Default | - | start-stop | PCSAUTHGRP | False |
| Commands - Default | all | start-stop | PCSAUTHGRP | False |

#### AAA Accounting Device Configuration

```eos
aaa accounting exec default start-stop group PCSAUTHGRP
aaa accounting commands all default start-stop group PCSAUTHGRP
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 10.113.5.1:9910,10.113.5.2:9910,10.113.5.3:9910 | PCS-NETINFRA-OOB | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | - | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.113.5.1:9910,10.113.5.2:9910,10.113.5.3:9910 -cvauth=token,/tmp/token -cvvrf=PCS-NETINFRA-OOB -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs -cvsourceintf=Management1
   no shutdown
```

### Logging

#### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |
| Console | warnings |
| Buffer | informational |

| Format Type | Setting |
| ----------- | ------- |
| Timestamp | high-resolution |
| Hostname | hostname |
| Sequence-numbers | false |
| RFC5424 | False |

| VRF | Source Interface |
| --- | ---------------- |
| - | Management1 |
| PCS-NETINFRA-OOB | Management1 |

| VRF | Hosts | Ports | Protocol | SSL-profile |
| --- | ----- | ----- | -------- | ----------- |
| PCS-NETINFRA-OOB | 10.115.4.100 | Default | UDP | - |

#### Logging Servers and Features Device Configuration

```eos
!
logging buffered 100000 informational
logging console warnings
logging vrf PCS-NETINFRA-OOB host 10.115.4.100
logging format timestamp high-resolution
logging source-interface Management1
logging vrf PCS-NETINFRA-OOB source-interface Management1
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| Saleem Nawaz Khan | KDC | All | Enabled |
| Saleem Nawaz Khan | KDC | bgp | Enabled |
| Saleem Nawaz Khan | KDC |  | Disabled |

#### SNMP EngineID Configuration

| Type | EngineID (Hex) | IP | Port |
| ---- | -------------- | -- | ---- |
| local | f5717ffc59c065f52900 | - | - |
| remote | 1234567890 | server1 | - |

#### SNMP ACLs

| IP | ACL | VRF |
| -- | --- | --- |
| IPv4 | ACL-SNMP | PCS-NETINFRA-OOB |

#### SNMP VRF Status

| VRF | Status |
| --- | ------ |
| PCS-NETINFRA-OOB | Enabled |

#### SNMP Hosts Configuration

| Host | VRF | Community | Username | Authentication level | SNMP Version |
| ---- |---- | --------- | -------- | -------------------- | ------------ |

#### SNMP Groups Configuration

| Group | SNMP Version | Authentication | Read | Write | Notify |
| ----- | ------------ | -------------- | ---- | ----- | ------ |
| group1 | v3 | priv | - | - | - |
| group2 | v3 | priv | - | - | - |

#### SNMP Users Configuration

| User | Group | Version | Authentication | Privacy | Remote Address | Remote Port | Engine ID |
| ---- | ----- | ------- | -------------- | ------- | -------------- | ----------- | --------- |
| SVC_ITOM.Entuity | group2 | v3 | sha256 | aes256 | - | - | f5717ffc59c065f52900 |

#### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list ACL-SNMP vrf PCS-NETINFRA-OOB
snmp-server engineID local f5717ffc59c065f52900
snmp-server contact Saleem Nawaz Khan
snmp-server location KDC
snmp-server group group1 v3 priv
snmp-server group group2 v3 priv
snmp-server user SVC_ITOM.Entuity group2 v3 localized f5717ffc59c065f52900 auth sha256 <removed> priv aes256 <removed>
snmp-server engineID remote server1 1234567890
snmp-server enable traps
snmp-server enable traps bgp
snmp-server vrf PCS-NETINFRA-OOB
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

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 1005 | INBAND_MGMT | - |

### VLANs Device Configuration

```eos
!
vlan 1005
   name INBAND_MGMT
```

## Interfaces

### Interface Defaults

#### Interface Defaults Summary

- Default Ethernet Interface Shutdown: True

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   ethernet
      shutdown
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet49 | L2_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/48/1 | *trunk | *1005 | *- | *- | 49 |
| Ethernet50 | L2_KDC-AR7508R3-PCS-FELEAF2_Ethernet6/48/1 | *trunk | *1005 | *- | *- | 49 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet49
   description L2_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/48/1
   no shutdown
   channel-group 49 mode active
!
interface Ethernet50
   description L2_KDC-AR7508R3-PCS-FELEAF2_Ethernet6/48/1
   no shutdown
   channel-group 49 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel49 | L2_FE_LEAF_Port-Channel1648 | trunk | 1005 | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel49
   description L2_FE_LEAF_Port-Channel1648
   no shutdown
   switchport trunk allowed vlan 1005
   switchport mode trunk
   switchport
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan1005 | PureCS_Network_Infra_nodes_OOB | PCS-NETINFRA-OOB | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan1005 |  PCS-NETINFRA-OOB  |  10.118.5.42/24  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan1005
   description PureCS_Network_Infra_nodes_OOB
   no shutdown
   mtu 1500
   vrf PCS-NETINFRA-OOB
   ip address 10.118.5.42/24
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

## Queue Monitor

### Queue Monitor Length

| Enabled | Logging Interval | Default Thresholds High | Default Thresholds Low | Notifying | TX Latency | CPU Thresholds High | CPU Thresholds Low | Mirroring Enabled | Mirror destinations |
| ------- | ---------------- | ----------------------- | ---------------------- | --------- | ---------- | ------------------- | ------------------ | ----------------- | ------------------ |
| True | 300 | - | - | disabled | disabled | - | - | - | - |

### Queue Monitor Streaming

| Enabled | IP Access Group | IPv6 Access Group | Max Connections | VRF |
| ------- | --------------- | ----------------- | --------------- | --- |
| True | - | - | - | - |

### Queue Monitor Configuration

```eos
!
queue-monitor length
!
queue-monitor length log 300
!
queue-monitor streaming
   no shutdown
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
