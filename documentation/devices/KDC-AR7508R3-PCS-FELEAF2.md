# KDC-AR7508R3-PCS-FELEAF2

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
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
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
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Queue Monitor](#queue-monitor)
  - [Queue Monitor Length](#queue-monitor-length)
  - [Queue Monitor Streaming](#queue-monitor-streaming)
  - [Queue Monitor Configuration](#queue-monitor-configuration)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
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
| Management1/1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | 10.118.5.17/24 | 10.118.5.254 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1/1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1/1
   description OOB_MANAGEMENT
   no shutdown
   vrf PCS-NETINFRA-OOB
   ip address 10.118.5.17/24
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
| Management1/1 | PCS-NETINFRA-OOB |

#### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf PCS-NETINFRA-OOB source-interface Management1/1
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
| PCS-NETINFRA-OOB | Management1/1 |

#### IP TACACS Source Interfaces Device Configuration

```eos
!
ip tacacs vrf PCS-NETINFRA-OOB source-interface Management1/1
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
| gzip | 10.113.5.1:9910,10.113.5.2:9910,10.113.5.3:9910 | PCS-NETINFRA-OOB | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | - | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.113.5.1:9910,10.113.5.2:9910,10.113.5.3:9910 -cvauth=token,/tmp/token -cvvrf=PCS-NETINFRA-OOB -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs -cvsourceintf=Management1/1
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| MLAG_FE_LEAF_01 | Vlan4094 | 10.118.4.254 | Port-Channel1000 |

Dual primary detection is enabled. The detection delay is 5 seconds.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id MLAG_FE_LEAF_01
   local-interface Vlan4094
   peer-address 10.118.4.254
   peer-address heartbeat 10.118.5.16 vrf PCS-NETINFRA-OOB
   peer-link Port-Channel1000
   dual-primary detection delay 5 action errdisable all-interfaces
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
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
| 5 | PureCS_Network_Infra_nodes_OOB | - |
| 7 | MPLS_Outside_DC_firewall | - |
| 8 | Internet_HIS_VDOM_outside_DC_Fir | - |
| 14 | PURE_VRF_MPLS | - |
| 17 | PCS_AAN_MPLS | - |
| 18 | EMAC-VLAN-For-Internet | - |
| 303 | SEHA_VPLS_HCF | - |
| 1000 | native_vlan | - |
| 1004 | PC-MGMT-MPLS | - |
| 1006 | MPLS_PC-MPLS_VDOM_outside_DC_FW | - |
| 1009 | Internet_PC-MGMT_VDOM_Outside_DC | - |
| 1010 | Internet_PC-WLD_VDOM_outside_DC_ | - |
| 1011 | PC-Shared-INT | - |
| 1012 | iDRAC-MPLS | - |
| 1013 | iDRAC-Internet | - |
| 1014 | PC-WLD-MPLS | - |
| 1015 | MPLS_Intranet_VDOM_Inside_P_FW | - |
| 1016 | Peri-shared-internetVDOM | - |
| 1017 | MPLS_Intranet_VDOM_Outside_P_FW_ | - |
| 1018 | Internet_Internet_VDOM_Outside_P | - |
| 1019 | PC-Shared-MPLS | - |
| 1020 | KDC-iDRAC-Pcloud | - |
| 1021 | pc-mgm-mgmt | - |
| 1022 | pcs-mgm-vmotion | - |
| 1023 | pcs-mgm-vSAN | - |
| 1024 | pcs-mgm-NSX-T_Host_Overlay | - |
| 1025 | pcs-mgm-Edge_Uplink_1 | - |
| 1026 | pcs-mgm-Edge_Uplink_2 | - |
| 1027 | pcs-mgm-Edge_Overlay | - |
| 1028 | ESXi-Host-MGMT | - |
| 1029 | PC_BACKUP | - |
| 1033 | Seha_CRS_Rep | - |
| 1035 | Seha_CRS_CROOM | - |
| 1036 | VAULT-OOB | - |
| 1037 | FlexAMgmt | - |
| 1038 | pcs-mgm-ReplicationVMs | - |
| 1050 | pcs-wld-oob | - |
| 1051 | pcs-wld-vmotion | - |
| 1052 | pcs-wld-vSAN | - |
| 1053 | pcs-wld-NSX-T_Host_Overlay | - |
| 1054 | pcs-wld-Edge_Uplink_1 | - |
| 1055 | pcs-wld-Edge_Uplink_2 | - |
| 1056 | pcs-wld-Edge_Overlay | - |
| 1057 | Nutanix-AHV-CVM | - |
| 1058 | PCS_CSE_SERVICES_ORG | - |
| 1060 | pcs-wld-vcda | - |
| 1061 | pcs-wld-Tenant1-Internet-U1 | - |
| 1062 | pcs-wld-Tenant1-MPLS-U1 | - |
| 1101 | pcs-idrac-management | - |
| 1102 | pcs-esxi-management | - |
| 1155 | pcs-his-backups | - |
| 1157 | VLAN1157 | - |
| 1160 | powerstore_NSF | - |
| 1161 | iSCSI_ControllerA | - |
| 1162 | iSCSI_ControllerB | - |
| 1163 | PC_ECS_MGMT | - |
| 1164 | PC_ECS_REPLICATION | - |
| 1165 | PC_ECS_DATA | - |
| 1166 | Dawak_Pura_Callcenter-SBCs | - |
| 1167 | Dawak_Pura_Callcenter-Edge | - |
| 1168 | PCS_DAWAK_CC_OOB_IDRAC | - |
| 1169 | VLAN1169 | - |
| 1200 | PC-DR_Shared_Domain_Zone | - |
| 1201 | PC-DR_Shared_MonBCKUP_Zone | - |
| 1202 | PC_Shared_EndSecurity_Zone | - |
| 1203 | PC_Shared_SOC_Zone | - |
| 1204 | PC-DR_Shared_JumpInfra_Zone | - |
| 1205 | PC_Shared_JumpExt_Zone | - |
| 1206 | VAULT-DMZ | - |
| 1301 | MPLS_NSOC | - |
| 1302 | SEHA_VPLS | - |
| 1304 | Connected_AL_AIN_MALAFI_MPLS | - |
| 1309 | SEHA_CRS_Rep | - |
| 1310 | SEHA_CRS_CLEANROOM | - |
| 1311 | Riayati_MPLS | - |
| 1350 | SD_WAN_TRANSIT | - |
| 1351 | SD_WAN_INTERNET | - |
| 1501 | mss_and_anycast_test | - |
| 2000 | DC_DC_L2 | - |
| 2005 | VAULT-OOB | - |
| 2006 | FlexAMgmt | - |
| 2007 | Nutanix-AHV-CVM | - |
| 2008 | VM-NuatanixProd | - |
| 2009 | Veritas-Daman-Rep | - |
| 2010 | Veritas-SEHA-Rep | - |
| 2011 | Veritas-Tenant-Rep | - |
| 2012 | Veritas-Daman-Mgmt | - |
| 2013 | Veritas-SEHA-Mgmt | - |
| 2014 | Scan-VMs | - |
| 2015 | JS-VLAN | - |
| 2016 | NTP-VLAN | - |
| 4091 | MLAG_L3_VRF_PCS-SHARED-INTERNET | MLAG |
| 4092 | MLAG_L3_VRF_PCS-MPLS-INTRANET | MLAG |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 5
   name PureCS_Network_Infra_nodes_OOB
!
vlan 7
   name MPLS_Outside_DC_firewall
!
vlan 8
   name Internet_HIS_VDOM_outside_DC_Fir
!
vlan 14
   name PURE_VRF_MPLS
!
vlan 17
   name PCS_AAN_MPLS
!
vlan 18
   name EMAC-VLAN-For-Internet
!
vlan 303
   name SEHA_VPLS_HCF
!
vlan 1000
   name native_vlan
!
vlan 1004
   name PC-MGMT-MPLS
!
vlan 1006
   name MPLS_PC-MPLS_VDOM_outside_DC_FW
!
vlan 1009
   name Internet_PC-MGMT_VDOM_Outside_DC
!
vlan 1010
   name Internet_PC-WLD_VDOM_outside_DC_
!
vlan 1011
   name PC-Shared-INT
!
vlan 1012
   name iDRAC-MPLS
!
vlan 1013
   name iDRAC-Internet
!
vlan 1014
   name PC-WLD-MPLS
!
vlan 1015
   name MPLS_Intranet_VDOM_Inside_P_FW
!
vlan 1016
   name Peri-shared-internetVDOM
!
vlan 1017
   name MPLS_Intranet_VDOM_Outside_P_FW_
!
vlan 1018
   name Internet_Internet_VDOM_Outside_P
!
vlan 1019
   name PC-Shared-MPLS
!
vlan 1020
   name KDC-iDRAC-Pcloud
!
vlan 1021
   name pc-mgm-mgmt
!
vlan 1022
   name pcs-mgm-vmotion
!
vlan 1023
   name pcs-mgm-vSAN
!
vlan 1024
   name pcs-mgm-NSX-T_Host_Overlay
!
vlan 1025
   name pcs-mgm-Edge_Uplink_1
!
vlan 1026
   name pcs-mgm-Edge_Uplink_2
!
vlan 1027
   name pcs-mgm-Edge_Overlay
!
vlan 1028
   name ESXi-Host-MGMT
!
vlan 1029
   name PC_BACKUP
!
vlan 1033
   name Seha_CRS_Rep
!
vlan 1035
   name Seha_CRS_CROOM
!
vlan 1036
   name VAULT-OOB
!
vlan 1037
   name FlexAMgmt
!
vlan 1038
   name pcs-mgm-ReplicationVMs
!
vlan 1050
   name pcs-wld-oob
!
vlan 1051
   name pcs-wld-vmotion
!
vlan 1052
   name pcs-wld-vSAN
!
vlan 1053
   name pcs-wld-NSX-T_Host_Overlay
!
vlan 1054
   name pcs-wld-Edge_Uplink_1
!
vlan 1055
   name pcs-wld-Edge_Uplink_2
!
vlan 1056
   name pcs-wld-Edge_Overlay
!
vlan 1057
   name Nutanix-AHV-CVM
!
vlan 1058
   name PCS_CSE_SERVICES_ORG
!
vlan 1060
   name pcs-wld-vcda
!
vlan 1061
   name pcs-wld-Tenant1-Internet-U1
!
vlan 1062
   name pcs-wld-Tenant1-MPLS-U1
!
vlan 1101
   name pcs-idrac-management
!
vlan 1102
   name pcs-esxi-management
!
vlan 1155
   name pcs-his-backups
!
vlan 1157
   name VLAN1157
!
vlan 1160
   name powerstore_NSF
!
vlan 1161
   name iSCSI_ControllerA
!
vlan 1162
   name iSCSI_ControllerB
!
vlan 1163
   name PC_ECS_MGMT
!
vlan 1164
   name PC_ECS_REPLICATION
!
vlan 1165
   name PC_ECS_DATA
!
vlan 1166
   name Dawak_Pura_Callcenter-SBCs
!
vlan 1167
   name Dawak_Pura_Callcenter-Edge
!
vlan 1168
   name PCS_DAWAK_CC_OOB_IDRAC
!
vlan 1169
   name VLAN1169
!
vlan 1200
   name PC-DR_Shared_Domain_Zone
!
vlan 1201
   name PC-DR_Shared_MonBCKUP_Zone
!
vlan 1202
   name PC_Shared_EndSecurity_Zone
!
vlan 1203
   name PC_Shared_SOC_Zone
!
vlan 1204
   name PC-DR_Shared_JumpInfra_Zone
!
vlan 1205
   name PC_Shared_JumpExt_Zone
!
vlan 1206
   name VAULT-DMZ
!
vlan 1301
   name MPLS_NSOC
!
vlan 1302
   name SEHA_VPLS
!
vlan 1304
   name Connected_AL_AIN_MALAFI_MPLS
!
vlan 1309
   name SEHA_CRS_Rep
!
vlan 1310
   name SEHA_CRS_CLEANROOM
!
vlan 1311
   name Riayati_MPLS
!
vlan 1350
   name SD_WAN_TRANSIT
!
vlan 1351
   name SD_WAN_INTERNET
!
vlan 1501
   name mss_and_anycast_test
!
vlan 2000
   name DC_DC_L2
!
vlan 2005
   name VAULT-OOB
!
vlan 2006
   name FlexAMgmt
!
vlan 2007
   name Nutanix-AHV-CVM
!
vlan 2008
   name VM-NuatanixProd
!
vlan 2009
   name Veritas-Daman-Rep
!
vlan 2010
   name Veritas-SEHA-Rep
!
vlan 2011
   name Veritas-Tenant-Rep
!
vlan 2012
   name Veritas-Daman-Mgmt
!
vlan 2013
   name Veritas-SEHA-Mgmt
!
vlan 2014
   name Scan-VMs
!
vlan 2015
   name JS-VLAN
!
vlan 2016
   name NTP-VLAN
!
vlan 4091
   name MLAG_L3_VRF_PCS-SHARED-INTERNET
   trunk group MLAG
!
vlan 4092
   name MLAG_L3_VRF_PCS-MPLS-INTRANET
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet3/21/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/21/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet3/22/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/22/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet3/23/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/23/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet3/24/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/24/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet5 | SERVER_dc1-leaf1-server1_PCI2 | *trunk | *11-12,21-22 | *1000 | *- | 5 |
| Ethernet6/40/1 | LOAD_BALANCER_01_4.0 | *trunk | *140 | *1000 | *- | 1640 |
| Ethernet6/41/1 | LOAD_BALANCER_01_6.0 | *trunk | *140 | *1000 | *- | 1640 |
| Ethernet6/42/1 | LOAD_BALANCER_02_4.0 | *trunk | *140 | *1000 | *- | 1642 |
| Ethernet6/43/1 | LOAD_BALANCER_02_6.0 | *trunk | *140 | *1000 | *- | 1642 |
| Ethernet6/44/1 | FIREWALL_DC-01_29 | *trunk | *1004-1006,1009,1011-1013,1025 | *1000 | *- | 1644 |
| Ethernet6/45/1 | FIREWALL_DC-02_29 | *trunk | *1004-1006,1009,1011-1013,1025 | *1000 | *- | 1645 |
| Ethernet6/46/1 | FIREWALL_perimerter-01_23 | *trunk | *15-16 | *1000 | *- | 1646 |
| Ethernet6/47/1 | L2_KDC-AR7010TX-PCS-OOB1_Ethernet50 | *trunk | *5 | *- | *- | 1647 |
| Ethernet6/48/1 | L2_KDC-AR7010TX-PCS-OOB2_Ethernet50 | *trunk | *5 | *- | *- | 1648 |
| Ethernet6/49/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/49/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet6/50/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/50/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet7/37/1 | L2_KDC-AR7010TX-PCS-OOB3_Ethernet50 | *trunk | *5 | *- | *- | 1637 |
| Ethernet7/40/1 | LOAD_BALANCER_01_8.0 | *trunk | *140 | *1000 | *- | 1640 |
| Ethernet7/41/1 | LOAD_BALANCER_01_10.0 | *trunk | *140 | *1000 | *- | 1640 |
| Ethernet7/42/1 | LOAD_BALANCER_02_8.0 | *trunk | *140 | *1000 | *- | 1642 |
| Ethernet7/43/1 | LOAD_BALANCER_02_10.0 | *trunk | *140 | *1000 | *- | 1642 |
| Ethernet7/44/1 | FIREWALL_DC-01_30 | *trunk | *1004-1006,1009,1011-1013,1025 | *1000 | *- | 1644 |
| Ethernet7/45/1 | FIREWALL_DC-02_30 | *trunk | *1004-1006,1009,1011-1013,1025 | *1000 | *- | 1645 |
| Ethernet7/46/1 | FIREWALL_perimerter-02_24 | *trunk | *15-16 | *1000 | *- | 1746 |
| Ethernet7/47/1 | ROUTER_Internet-CPE-02_2 | access | 18 | - | - | - |
| Ethernet7/48/1 | ROUTER_MPLS-CPE-02_5 | access | 17 | - | - | - |
| Ethernet7/49/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet7/49/1 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet7/50/1 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet7/50/1 | *trunk | *- | *- | *MLAG | 1000 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet3/21/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/21/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet3/22/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/22/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet3/23/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/23/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet3/24/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet3/24/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet5
   description SERVER_dc1-leaf1-server1_PCI2
   no shutdown
   channel-group 5 mode active
!
interface Ethernet6/40/1
   description LOAD_BALANCER_01_4.0
   no shutdown
   channel-group 1640 mode active
!
interface Ethernet6/41/1
   description LOAD_BALANCER_01_6.0
   no shutdown
   channel-group 1640 mode active
!
interface Ethernet6/42/1
   description LOAD_BALANCER_02_4.0
   no shutdown
   channel-group 1642 mode active
!
interface Ethernet6/43/1
   description LOAD_BALANCER_02_6.0
   no shutdown
   channel-group 1642 mode active
!
interface Ethernet6/44/1
   description FIREWALL_DC-01_29
   no shutdown
   channel-group 1644 mode active
!
interface Ethernet6/45/1
   description FIREWALL_DC-02_29
   no shutdown
   channel-group 1645 mode active
!
interface Ethernet6/46/1
   description FIREWALL_perimerter-01_23
   no shutdown
   channel-group 1646 mode active
!
interface Ethernet6/47/1
   description L2_KDC-AR7010TX-PCS-OOB1_Ethernet50
   no shutdown
   channel-group 1647 mode active
!
interface Ethernet6/48/1
   description L2_KDC-AR7010TX-PCS-OOB2_Ethernet50
   no shutdown
   channel-group 1648 mode active
!
interface Ethernet6/49/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/49/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet6/50/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet6/50/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet7/37/1
   description L2_KDC-AR7010TX-PCS-OOB3_Ethernet50
   no shutdown
   channel-group 1637 mode active
!
interface Ethernet7/40/1
   description LOAD_BALANCER_01_8.0
   no shutdown
   channel-group 1640 mode active
!
interface Ethernet7/41/1
   description LOAD_BALANCER_01_10.0
   no shutdown
   channel-group 1640 mode active
!
interface Ethernet7/42/1
   description LOAD_BALANCER_02_8.0
   no shutdown
   channel-group 1642 mode active
!
interface Ethernet7/43/1
   description LOAD_BALANCER_02_10.0
   no shutdown
   channel-group 1642 mode active
!
interface Ethernet7/44/1
   description FIREWALL_DC-01_30
   no shutdown
   channel-group 1644 mode active
!
interface Ethernet7/45/1
   description FIREWALL_DC-02_30
   no shutdown
   channel-group 1645 mode active
!
interface Ethernet7/46/1
   description FIREWALL_perimerter-02_24
   no shutdown
   channel-group 1746 mode active
!
interface Ethernet7/47/1
   description ROUTER_Internet-CPE-02_2
   no shutdown
   switchport access vlan 18
   switchport mode access
   switchport
!
interface Ethernet7/48/1
   description ROUTER_MPLS-CPE-02_5
   no shutdown
   switchport access vlan 17
   switchport mode access
   switchport
!
interface Ethernet7/49/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet7/49/1
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet7/50/1
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Ethernet7/50/1
   no shutdown
   channel-group 1000 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel5 | SERVER_dc1-leaf1-server1_Bond1 | trunk | 11-12,21-22 | 1000 | - | - | - | 5 | - |
| Port-Channel1000 | MLAG_KDC-AR7508R3-PCS-FELEAF1_Port-Channel1000 | trunk | - | - | MLAG | - | - | - | - |
| Port-Channel1637 | L2_KDC-AR7010TX-PCS-OOB3_Port-Channel49 | trunk | 5 | - | - | - | - | 1637 | - |
| Port-Channel1640 | LOAD_BALANCER_01_Bond1 | trunk | 140 | 1000 | - | - | - | 1640 | - |
| Port-Channel1642 | LOAD_BALANCER_02_Bond1 | trunk | 140 | 1000 | - | - | - | 1642 | - |
| Port-Channel1644 | FIREWALL_DC-01_Bond1 | trunk | 1004-1006,1009,1011-1013,1025 | 1000 | - | - | - | 1644 | - |
| Port-Channel1645 | FIREWALL_DC-02_Bond1 | trunk | 1004-1006,1009,1011-1013,1025 | 1000 | - | - | - | 1645 | - |
| Port-Channel1646 | FIREWALL_perimerter-01_Bond1 | trunk | 15-16 | 1000 | - | - | - | 1646 | - |
| Port-Channel1647 | L2_KDC-AR7010TX-PCS-OOB1_Port-Channel49 | trunk | 5 | - | - | - | - | 1647 | - |
| Port-Channel1648 | L2_KDC-AR7010TX-PCS-OOB2_Port-Channel49 | trunk | 5 | - | - | - | - | 1648 | - |
| Port-Channel1746 | FIREWALL_perimerter-02_Bond1 | trunk | 15-16 | 1000 | - | - | - | 1746 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel5
   description SERVER_dc1-leaf1-server1_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 11-12,21-22
   switchport mode trunk
   switchport
   mlag 5
   spanning-tree portfast
!
interface Port-Channel1000
   description MLAG_KDC-AR7508R3-PCS-FELEAF1_Port-Channel1000
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
!
interface Port-Channel1637
   description L2_KDC-AR7010TX-PCS-OOB3_Port-Channel49
   no shutdown
   switchport trunk allowed vlan 5
   switchport mode trunk
   switchport
   mlag 1637
!
interface Port-Channel1640
   description LOAD_BALANCER_01_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 140
   switchport mode trunk
   switchport
   mlag 1640
!
interface Port-Channel1642
   description LOAD_BALANCER_02_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 140
   switchport mode trunk
   switchport
   mlag 1642
!
interface Port-Channel1644
   description FIREWALL_DC-01_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 1004-1006,1009,1011-1013,1025
   switchport mode trunk
   switchport
   mlag 1644
!
interface Port-Channel1645
   description FIREWALL_DC-02_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 1004-1006,1009,1011-1013,1025
   switchport mode trunk
   switchport
   mlag 1645
!
interface Port-Channel1646
   description FIREWALL_perimerter-01_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 15,16
   switchport mode trunk
   switchport
   mlag 1646
!
interface Port-Channel1647
   description L2_KDC-AR7010TX-PCS-OOB1_Port-Channel49
   no shutdown
   switchport trunk allowed vlan 5
   switchport mode trunk
   switchport
   mlag 1647
!
interface Port-Channel1648
   description L2_KDC-AR7010TX-PCS-OOB2_Port-Channel49
   no shutdown
   switchport trunk allowed vlan 5
   switchport mode trunk
   switchport
   mlag 1648
!
interface Port-Channel1746
   description FIREWALL_perimerter-02_Bond1
   no shutdown
   switchport trunk native vlan 1000
   switchport trunk allowed vlan 15,16
   switchport mode trunk
   switchport
   mlag 1746
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.118.0.4/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.118.1.3/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.118.0.4/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.118.1.3/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan1004 | PC-MGMT-MPLS | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1006 | PC-MPLS | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1009 | PC-MGMT-INT | PCS-SHARED-INTERNET | 9200 | False |
| Vlan1010 | PC-WLD-INT | PCS-SHARED-INTERNET | 9200 | False |
| Vlan1011 | PC-Shared-INT | PCS-SHARED-INTERNET | 9200 | False |
| Vlan1012 | PC-MPLS | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1013 | KDC iDRAC-to-Internet | PCS-SHARED-INTERNET | 9200 | False |
| Vlan1014 | PC-WLD-MPLS | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1015 | MPLS_Intranet_VDOM_Inside_P_FW | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1016 | Peri-shared-internetVDOM | PCS-SHARED-INTERNET | 9200 | False |
| Vlan1019 | PC-Shared-MPLS | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1022 | vMotion | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1023 | vSAN | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1024 | Management Host TEPs | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1027 | Management Edge TEPs | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1051 | Resource vMotion | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1053 | Resource Host TEPs | PCS-MPLS-INTRANET | 9200 | False |
| Vlan1056 | Resource Edge TEPs | PCS-MPLS-INTRANET | 9200 | False |
| Vlan4091 | MLAG_L3_VRF_PCS-SHARED-INTERNET | PCS-SHARED-INTERNET | 9214 | False |
| Vlan4092 | MLAG_L3_VRF_PCS-MPLS-INTRANET | PCS-MPLS-INTRANET | 9214 | False |
| Vlan4093 | MLAG_L3 | default | 9214 | False |
| Vlan4094 | MLAG | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan1004 |  PCS-MPLS-INTRANET  |  10.118.2.133/29  |  -  |  10.118.2.134  |  -  |  -  |
| Vlan1006 |  PCS-MPLS-INTRANET  |  10.118.2.21/29  |  -  |  10.118.2.22  |  -  |  -  |
| Vlan1009 |  PCS-SHARED-INTERNET  |  10.118.2.29/29  |  -  |  10.118.2.30  |  -  |  -  |
| Vlan1010 |  PCS-SHARED-INTERNET  |  10.118.2.37/29  |  -  |  10.118.2.38  |  -  |  -  |
| Vlan1011 |  PCS-SHARED-INTERNET  |  10.118.2.53/29  |  -  |  10.118.2.54  |  -  |  -  |
| Vlan1012 |  PCS-MPLS-INTRANET  |  10.118.2.61/29  |  -  |  10.118.2.62  |  -  |  -  |
| Vlan1013 |  PCS-SHARED-INTERNET  |  10.118.2.101/29  |  -  |  10.118.2.102  |  -  |  -  |
| Vlan1014 |  PCS-MPLS-INTRANET  |  10.118.2.141/29  |  -  |  10.118.2.142  |  -  |  -  |
| Vlan1015 |  PCS-MPLS-INTRANET  |  10.118.2.69/29  |  -  |  10.118.2.70  |  -  |  -  |
| Vlan1016 |  PCS-SHARED-INTERNET  |  10.118.2.77/29  |  -  |  10.118.2.78  |  -  |  -  |
| Vlan1019 |  PCS-MPLS-INTRANET  |  10.118.2.117/29  |  -  |  10.118.2.118  |  -  |  -  |
| Vlan1022 |  PCS-MPLS-INTRANET  |  10.118.22.253/24  |  -  |  10.118.22.254  |  -  |  -  |
| Vlan1023 |  PCS-MPLS-INTRANET  |  10.118.23.253/24  |  -  |  10.118.23.254  |  -  |  -  |
| Vlan1024 |  PCS-MPLS-INTRANET  |  10.118.24.253/24  |  -  |  10.118.24.254  |  -  |  -  |
| Vlan1027 |  PCS-MPLS-INTRANET  |  10.118.27.253/24  |  -  |  10.118.27.254  |  -  |  -  |
| Vlan1051 |  PCS-MPLS-INTRANET  |  10.118.51.253/24  |  -  |  10.118.51.254  |  -  |  -  |
| Vlan1053 |  PCS-MPLS-INTRANET  |  10.118.53.253/24  |  -  |  10.118.53.254  |  -  |  -  |
| Vlan1056 |  PCS-MPLS-INTRANET  |  10.118.56.253/24  |  -  |  10.118.56.254  |  -  |  -  |
| Vlan4091 |  PCS-SHARED-INTERNET  |  10.118.4.249/31  |  -  |  -  |  -  |  -  |
| Vlan4092 |  PCS-MPLS-INTRANET  |  10.118.4.251/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.118.4.253/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.118.4.255/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan1004
   description PC-MGMT-MPLS
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.133/29
   ip virtual-router address 10.118.2.134
!
interface Vlan1006
   description PC-MPLS
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.21/29
   ip virtual-router address 10.118.2.22
!
interface Vlan1009
   description PC-MGMT-INT
   no shutdown
   mtu 9200
   vrf PCS-SHARED-INTERNET
   ip address 10.118.2.29/29
   ip virtual-router address 10.118.2.30
!
interface Vlan1010
   description PC-WLD-INT
   no shutdown
   mtu 9200
   vrf PCS-SHARED-INTERNET
   ip address 10.118.2.37/29
   ip virtual-router address 10.118.2.38
!
interface Vlan1011
   description PC-Shared-INT
   no shutdown
   mtu 9200
   vrf PCS-SHARED-INTERNET
   ip address 10.118.2.53/29
   ip virtual-router address 10.118.2.54
!
interface Vlan1012
   description PC-MPLS
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.61/29
   ip virtual-router address 10.118.2.62
!
interface Vlan1013
   description KDC iDRAC-to-Internet
   no shutdown
   mtu 9200
   vrf PCS-SHARED-INTERNET
   ip address 10.118.2.101/29
   ip virtual-router address 10.118.2.102
!
interface Vlan1014
   description PC-WLD-MPLS
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.141/29
   ip virtual-router address 10.118.2.142
!
interface Vlan1015
   description MPLS_Intranet_VDOM_Inside_P_FW
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.69/29
   ip virtual-router address 10.118.2.70
!
interface Vlan1016
   description Peri-shared-internetVDOM
   no shutdown
   mtu 9200
   vrf PCS-SHARED-INTERNET
   ip address 10.118.2.77/29
   ip virtual-router address 10.118.2.78
!
interface Vlan1019
   description PC-Shared-MPLS
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.2.117/29
   ip virtual-router address 10.118.2.118
!
interface Vlan1022
   description vMotion
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.22.253/24
   ip virtual-router address 10.118.22.254
!
interface Vlan1023
   description vSAN
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.23.253/24
   ip virtual-router address 10.118.23.254
!
interface Vlan1024
   description Management Host TEPs
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.24.253/24
   ip virtual-router address 10.118.24.254
!
interface Vlan1027
   description Management Edge TEPs
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.27.253/24
   ip virtual-router address 10.118.27.254
!
interface Vlan1051
   description Resource vMotion
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.51.253/24
   ip virtual-router address 10.118.51.254
!
interface Vlan1053
   description Resource Host TEPs
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.53.253/24
   ip virtual-router address 10.118.53.254
!
interface Vlan1056
   description Resource Edge TEPs
   no shutdown
   mtu 9200
   vrf PCS-MPLS-INTRANET
   ip address 10.118.56.253/24
   ip virtual-router address 10.118.56.254
!
interface Vlan4091
   description MLAG_L3_VRF_PCS-SHARED-INTERNET
   no shutdown
   mtu 9214
   vrf PCS-SHARED-INTERNET
   ip address 10.118.4.249/31
!
interface Vlan4092
   description MLAG_L3_VRF_PCS-MPLS-INTRANET
   no shutdown
   mtu 9214
   vrf PCS-MPLS-INTRANET
   ip address 10.118.4.251/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 9214
   ip address 10.118.4.253/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 9214
   no autostate
   ip address 10.118.4.255/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 5 | 10005 | - | - |
| 7 | 10007 | - | - |
| 8 | 10008 | - | - |
| 14 | 10014 | - | - |
| 17 | 10017 | - | - |
| 18 | 10018 | - | - |
| 303 | 10303 | - | - |
| 1000 | 11000 | - | - |
| 1004 | 11004 | - | - |
| 1006 | 11006 | - | - |
| 1009 | 11009 | - | - |
| 1010 | 11010 | - | - |
| 1011 | 11011 | - | - |
| 1012 | 11012 | - | - |
| 1013 | 11013 | - | - |
| 1014 | 11014 | - | - |
| 1015 | 11015 | - | - |
| 1016 | 11016 | - | - |
| 1017 | 11017 | - | - |
| 1018 | 11018 | - | - |
| 1019 | 11019 | - | - |
| 1020 | 11020 | - | - |
| 1021 | 11021 | - | - |
| 1022 | 11022 | - | - |
| 1023 | 11023 | - | - |
| 1024 | 11024 | - | - |
| 1025 | 11025 | - | - |
| 1026 | 11026 | - | - |
| 1027 | 11027 | - | - |
| 1028 | 11028 | - | - |
| 1029 | 11029 | - | - |
| 1033 | 11033 | - | - |
| 1035 | 11035 | - | - |
| 1036 | 11036 | - | - |
| 1037 | 11037 | - | - |
| 1038 | 11038 | - | - |
| 1050 | 11050 | - | - |
| 1051 | 11051 | - | - |
| 1052 | 11052 | - | - |
| 1053 | 11053 | - | - |
| 1054 | 11054 | - | - |
| 1055 | 11055 | - | - |
| 1056 | 11056 | - | - |
| 1057 | 11057 | - | - |
| 1058 | 11058 | - | - |
| 1060 | 11060 | - | - |
| 1061 | 11061 | - | - |
| 1062 | 11062 | - | - |
| 1101 | 11101 | - | - |
| 1102 | 11102 | - | - |
| 1155 | 11155 | - | - |
| 1157 | 11157 | - | - |
| 1160 | 11160 | - | - |
| 1161 | 11161 | - | - |
| 1162 | 11162 | - | - |
| 1163 | 11163 | - | - |
| 1164 | 11164 | - | - |
| 1165 | 11165 | - | - |
| 1166 | 11166 | - | - |
| 1167 | 11167 | - | - |
| 1168 | 11168 | - | - |
| 1169 | 11169 | - | - |
| 1200 | 11200 | - | - |
| 1201 | 11201 | - | - |
| 1202 | 11202 | - | - |
| 1203 | 11203 | - | - |
| 1204 | 11204 | - | - |
| 1205 | 11205 | - | - |
| 1206 | 11206 | - | - |
| 1301 | 11301 | - | - |
| 1302 | 11302 | - | - |
| 1304 | 11304 | - | - |
| 1309 | 11309 | - | - |
| 1310 | 11310 | - | - |
| 1311 | 11311 | - | - |
| 1350 | 11350 | - | - |
| 1351 | 11351 | - | - |
| 1501 | 11501 | - | - |
| 2000 | 12000 | - | - |
| 2005 | 12005 | - | - |
| 2006 | 12006 | - | - |
| 2007 | 12007 | - | - |
| 2008 | 12008 | - | - |
| 2009 | 12009 | - | - |
| 2010 | 12010 | - | - |
| 2011 | 12011 | - | - |
| 2012 | 12012 | - | - |
| 2013 | 12013 | - | - |
| 2014 | 12014 | - | - |
| 2015 | 12015 | - | - |
| 2016 | 12016 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Overlay Multicast Group to Encap Mappings |
| --- | --- | ----------------------------------------- |
| PCS-MPLS-INTRANET | 10 | - |
| PCS-SHARED-INTERNET | 11 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description KDC-AR7508R3-PCS-FELEAF2_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 5 vni 10005
   vxlan vlan 7 vni 10007
   vxlan vlan 8 vni 10008
   vxlan vlan 14 vni 10014
   vxlan vlan 17 vni 10017
   vxlan vlan 18 vni 10018
   vxlan vlan 303 vni 10303
   vxlan vlan 1000 vni 11000
   vxlan vlan 1004 vni 11004
   vxlan vlan 1006 vni 11006
   vxlan vlan 1009 vni 11009
   vxlan vlan 1010 vni 11010
   vxlan vlan 1011 vni 11011
   vxlan vlan 1012 vni 11012
   vxlan vlan 1013 vni 11013
   vxlan vlan 1014 vni 11014
   vxlan vlan 1015 vni 11015
   vxlan vlan 1016 vni 11016
   vxlan vlan 1017 vni 11017
   vxlan vlan 1018 vni 11018
   vxlan vlan 1019 vni 11019
   vxlan vlan 1020 vni 11020
   vxlan vlan 1021 vni 11021
   vxlan vlan 1022 vni 11022
   vxlan vlan 1023 vni 11023
   vxlan vlan 1024 vni 11024
   vxlan vlan 1025 vni 11025
   vxlan vlan 1026 vni 11026
   vxlan vlan 1027 vni 11027
   vxlan vlan 1028 vni 11028
   vxlan vlan 1029 vni 11029
   vxlan vlan 1033 vni 11033
   vxlan vlan 1035 vni 11035
   vxlan vlan 1036 vni 11036
   vxlan vlan 1037 vni 11037
   vxlan vlan 1038 vni 11038
   vxlan vlan 1050 vni 11050
   vxlan vlan 1051 vni 11051
   vxlan vlan 1052 vni 11052
   vxlan vlan 1053 vni 11053
   vxlan vlan 1054 vni 11054
   vxlan vlan 1055 vni 11055
   vxlan vlan 1056 vni 11056
   vxlan vlan 1057 vni 11057
   vxlan vlan 1058 vni 11058
   vxlan vlan 1060 vni 11060
   vxlan vlan 1061 vni 11061
   vxlan vlan 1062 vni 11062
   vxlan vlan 1101 vni 11101
   vxlan vlan 1102 vni 11102
   vxlan vlan 1155 vni 11155
   vxlan vlan 1157 vni 11157
   vxlan vlan 1160 vni 11160
   vxlan vlan 1161 vni 11161
   vxlan vlan 1162 vni 11162
   vxlan vlan 1163 vni 11163
   vxlan vlan 1164 vni 11164
   vxlan vlan 1165 vni 11165
   vxlan vlan 1166 vni 11166
   vxlan vlan 1167 vni 11167
   vxlan vlan 1168 vni 11168
   vxlan vlan 1169 vni 11169
   vxlan vlan 1200 vni 11200
   vxlan vlan 1201 vni 11201
   vxlan vlan 1202 vni 11202
   vxlan vlan 1203 vni 11203
   vxlan vlan 1204 vni 11204
   vxlan vlan 1205 vni 11205
   vxlan vlan 1206 vni 11206
   vxlan vlan 1301 vni 11301
   vxlan vlan 1302 vni 11302
   vxlan vlan 1304 vni 11304
   vxlan vlan 1309 vni 11309
   vxlan vlan 1310 vni 11310
   vxlan vlan 1311 vni 11311
   vxlan vlan 1350 vni 11350
   vxlan vlan 1351 vni 11351
   vxlan vlan 1501 vni 11501
   vxlan vlan 2000 vni 12000
   vxlan vlan 2005 vni 12005
   vxlan vlan 2006 vni 12006
   vxlan vlan 2007 vni 12007
   vxlan vlan 2008 vni 12008
   vxlan vlan 2009 vni 12009
   vxlan vlan 2010 vni 12010
   vxlan vlan 2011 vni 12011
   vxlan vlan 2012 vni 12012
   vxlan vlan 2013 vni 12013
   vxlan vlan 2014 vni 12014
   vxlan vlan 2015 vni 12015
   vxlan vlan 2016 vni 12016
   vxlan vrf PCS-MPLS-INTRANET vni 10
   vxlan vrf PCS-SHARED-INTERNET vni 11
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:00:99

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:99
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| PCS-MPLS-INTRANET | True |
| PCS-NETINFRA-OOB | False |
| PCS-SHARED-INTERNET | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf PCS-MPLS-INTRANET
no ip routing vrf PCS-NETINFRA-OOB
ip routing vrf PCS-SHARED-INTERNET
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| PCS-MPLS-INTRANET | false |
| PCS-NETINFRA-OOB | false |
| PCS-SHARED-INTERNET | false |

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
| 65371 | 10.118.0.4 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
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

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65371 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.118.4.252 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.0.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.0.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.118.4.250 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | PCS-MPLS-INTRANET | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.118.4.248 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | PCS-SHARED-INTERNET | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Peer-tag In | Peer-tag Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ----------- | ------------ | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True | - | - | - | - | default | - |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 5 | 10.118.0.4:10005 | 10005:10005 | - | - | learned |
| 7 | 10.118.0.4:10007 | 10007:10007 | - | - | learned |
| 8 | 10.118.0.4:10008 | 10008:10008 | - | - | learned |
| 14 | 10.118.0.4:10014 | 10014:10014 | - | - | learned |
| 17 | 10.118.0.4:10017 | 10017:10017 | - | - | learned |
| 18 | 10.118.0.4:10018 | 10018:10018 | - | - | learned |
| 303 | 10.118.0.4:10303 | 10303:10303 | - | - | learned |
| 1000 | 10.118.0.4:11000 | 11000:11000 | - | - | learned |
| 1004 | 10.118.0.4:11004 | 11004:11004 | - | - | learned |
| 1006 | 10.118.0.4:11006 | 11006:11006 | - | - | learned |
| 1009 | 10.118.0.4:11009 | 11009:11009 | - | - | learned |
| 1010 | 10.118.0.4:11010 | 11010:11010 | - | - | learned |
| 1011 | 10.118.0.4:11011 | 11011:11011 | - | - | learned |
| 1012 | 10.118.0.4:11012 | 11012:11012 | - | - | learned |
| 1013 | 10.118.0.4:11013 | 11013:11013 | - | - | learned |
| 1014 | 10.118.0.4:11014 | 11014:11014 | - | - | learned |
| 1015 | 10.118.0.4:11015 | 11015:11015 | - | - | learned |
| 1016 | 10.118.0.4:11016 | 11016:11016 | - | - | learned |
| 1017 | 10.118.0.4:11017 | 11017:11017 | - | - | learned |
| 1018 | 10.118.0.4:11018 | 11018:11018 | - | - | learned |
| 1019 | 10.118.0.4:11019 | 11019:11019 | - | - | learned |
| 1020 | 10.118.0.4:11020 | 11020:11020 | - | - | learned |
| 1021 | 10.118.0.4:11021 | 11021:11021 | - | - | learned |
| 1022 | 10.118.0.4:11022 | 11022:11022 | - | - | learned |
| 1023 | 10.118.0.4:11023 | 11023:11023 | - | - | learned |
| 1024 | 10.118.0.4:11024 | 11024:11024 | - | - | learned |
| 1025 | 10.118.0.4:11025 | 11025:11025 | - | - | learned |
| 1026 | 10.118.0.4:11026 | 11026:11026 | - | - | learned |
| 1027 | 10.118.0.4:11027 | 11027:11027 | - | - | learned |
| 1028 | 10.118.0.4:11028 | 11028:11028 | - | - | learned |
| 1029 | 10.118.0.4:11029 | 11029:11029 | - | - | learned |
| 1033 | 10.118.0.4:11033 | 11033:11033 | - | - | learned |
| 1035 | 10.118.0.4:11035 | 11035:11035 | - | - | learned |
| 1036 | 10.118.0.4:11036 | 11036:11036 | - | - | learned |
| 1037 | 10.118.0.4:11037 | 11037:11037 | - | - | learned |
| 1038 | 10.118.0.4:11038 | 11038:11038 | - | - | learned |
| 1050 | 10.118.0.4:11050 | 11050:11050 | - | - | learned |
| 1051 | 10.118.0.4:11051 | 11051:11051 | - | - | learned |
| 1052 | 10.118.0.4:11052 | 11052:11052 | - | - | learned |
| 1053 | 10.118.0.4:11053 | 11053:11053 | - | - | learned |
| 1054 | 10.118.0.4:11054 | 11054:11054 | - | - | learned |
| 1055 | 10.118.0.4:11055 | 11055:11055 | - | - | learned |
| 1056 | 10.118.0.4:11056 | 11056:11056 | - | - | learned |
| 1057 | 10.118.0.4:11057 | 11057:11057 | - | - | learned |
| 1058 | 10.118.0.4:11058 | 11058:11058 | - | - | learned |
| 1060 | 10.118.0.4:11060 | 11060:11060 | - | - | learned |
| 1061 | 10.118.0.4:11061 | 11061:11061 | - | - | learned |
| 1062 | 10.118.0.4:11062 | 11062:11062 | - | - | learned |
| 1101 | 10.118.0.4:11101 | 11101:11101 | - | - | learned |
| 1102 | 10.118.0.4:11102 | 11102:11102 | - | - | learned |
| 1155 | 10.118.0.4:11155 | 11155:11155 | - | - | learned |
| 1157 | 10.118.0.4:11157 | 11157:11157 | - | - | learned |
| 1160 | 10.118.0.4:11160 | 11160:11160 | - | - | learned |
| 1161 | 10.118.0.4:11161 | 11161:11161 | - | - | learned |
| 1162 | 10.118.0.4:11162 | 11162:11162 | - | - | learned |
| 1163 | 10.118.0.4:11163 | 11163:11163 | - | - | learned |
| 1164 | 10.118.0.4:11164 | 11164:11164 | - | - | learned |
| 1165 | 10.118.0.4:11165 | 11165:11165 | - | - | learned |
| 1166 | 10.118.0.4:11166 | 11166:11166 | - | - | learned |
| 1167 | 10.118.0.4:11167 | 11167:11167 | - | - | learned |
| 1168 | 10.118.0.4:11168 | 11168:11168 | - | - | learned |
| 1169 | 10.118.0.4:11169 | 11169:11169 | - | - | learned |
| 1200 | 10.118.0.4:11200 | 11200:11200 | - | - | learned |
| 1201 | 10.118.0.4:11201 | 11201:11201 | - | - | learned |
| 1202 | 10.118.0.4:11202 | 11202:11202 | - | - | learned |
| 1203 | 10.118.0.4:11203 | 11203:11203 | - | - | learned |
| 1204 | 10.118.0.4:11204 | 11204:11204 | - | - | learned |
| 1205 | 10.118.0.4:11205 | 11205:11205 | - | - | learned |
| 1206 | 10.118.0.4:11206 | 11206:11206 | - | - | learned |
| 1301 | 10.118.0.4:11301 | 11301:11301 | - | - | learned |
| 1302 | 10.118.0.4:11302 | 11302:11302 | - | - | learned |
| 1304 | 10.118.0.4:11304 | 11304:11304 | - | - | learned |
| 1309 | 10.118.0.4:11309 | 11309:11309 | - | - | learned |
| 1310 | 10.118.0.4:11310 | 11310:11310 | - | - | learned |
| 1311 | 10.118.0.4:11311 | 11311:11311 | - | - | learned |
| 1350 | 10.118.0.4:11350 | 11350:11350 | - | - | learned |
| 1351 | 10.118.0.4:11351 | 11351:11351 | - | - | learned |
| 1501 | 10.118.0.4:11501 | 11501:11501 | - | - | learned |
| 2000 | 10.118.0.4:12000 | 12000:12000 | - | - | learned |
| 2005 | 10.118.0.4:12005 | 12005:12005 | - | - | learned |
| 2006 | 10.118.0.4:12006 | 12006:12006 | - | - | learned |
| 2007 | 10.118.0.4:12007 | 12007:12007 | - | - | learned |
| 2008 | 10.118.0.4:12008 | 12008:12008 | - | - | learned |
| 2009 | 10.118.0.4:12009 | 12009:12009 | - | - | learned |
| 2010 | 10.118.0.4:12010 | 12010:12010 | - | - | learned |
| 2011 | 10.118.0.4:12011 | 12011:12011 | - | - | learned |
| 2012 | 10.118.0.4:12012 | 12012:12012 | - | - | learned |
| 2013 | 10.118.0.4:12013 | 12013:12013 | - | - | learned |
| 2014 | 10.118.0.4:12014 | 12014:12014 | - | - | learned |
| 2015 | 10.118.0.4:12015 | 12015:12015 | - | - | learned |
| 2016 | 10.118.0.4:12016 | 12016:12016 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| PCS-MPLS-INTRANET | 10.118.0.4:10 | connected | - |
| PCS-SHARED-INTERNET | 10.118.0.4:11 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65371
   router-id 10.118.0.4
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
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
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65371
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description KDC-AR7508R3-PCS-FELEAF1
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.118.4.252 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.118.4.252 description KDC-AR7508R3-PCS-FELEAF1_Vlan4093
   neighbor 10.255.0.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.1 remote-as 65100
   neighbor 10.255.0.1 description dc1-spine1_Loopback0
   neighbor 10.255.0.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.2 remote-as 65100
   neighbor 10.255.0.2 description dc1-spine2_Loopback0
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 5
      rd 10.118.0.4:10005
      route-target both 10005:10005
      redistribute learned
   !
   vlan 7
      rd 10.118.0.4:10007
      route-target both 10007:10007
      redistribute learned
   !
   vlan 8
      rd 10.118.0.4:10008
      route-target both 10008:10008
      redistribute learned
   !
   vlan 14
      rd 10.118.0.4:10014
      route-target both 10014:10014
      redistribute learned
   !
   vlan 17
      rd 10.118.0.4:10017
      route-target both 10017:10017
      redistribute learned
   !
   vlan 18
      rd 10.118.0.4:10018
      route-target both 10018:10018
      redistribute learned
   !
   vlan 303
      rd 10.118.0.4:10303
      route-target both 10303:10303
      redistribute learned
   !
   vlan 1000
      rd 10.118.0.4:11000
      route-target both 11000:11000
      redistribute learned
   !
   vlan 1004
      rd 10.118.0.4:11004
      route-target both 11004:11004
      redistribute learned
   !
   vlan 1006
      rd 10.118.0.4:11006
      route-target both 11006:11006
      redistribute learned
   !
   vlan 1009
      rd 10.118.0.4:11009
      route-target both 11009:11009
      redistribute learned
   !
   vlan 1010
      rd 10.118.0.4:11010
      route-target both 11010:11010
      redistribute learned
   !
   vlan 1011
      rd 10.118.0.4:11011
      route-target both 11011:11011
      redistribute learned
   !
   vlan 1012
      rd 10.118.0.4:11012
      route-target both 11012:11012
      redistribute learned
   !
   vlan 1013
      rd 10.118.0.4:11013
      route-target both 11013:11013
      redistribute learned
   !
   vlan 1014
      rd 10.118.0.4:11014
      route-target both 11014:11014
      redistribute learned
   !
   vlan 1015
      rd 10.118.0.4:11015
      route-target both 11015:11015
      redistribute learned
   !
   vlan 1016
      rd 10.118.0.4:11016
      route-target both 11016:11016
      redistribute learned
   !
   vlan 1017
      rd 10.118.0.4:11017
      route-target both 11017:11017
      redistribute learned
   !
   vlan 1018
      rd 10.118.0.4:11018
      route-target both 11018:11018
      redistribute learned
   !
   vlan 1019
      rd 10.118.0.4:11019
      route-target both 11019:11019
      redistribute learned
   !
   vlan 1020
      rd 10.118.0.4:11020
      route-target both 11020:11020
      redistribute learned
   !
   vlan 1021
      rd 10.118.0.4:11021
      route-target both 11021:11021
      redistribute learned
   !
   vlan 1022
      rd 10.118.0.4:11022
      route-target both 11022:11022
      redistribute learned
   !
   vlan 1023
      rd 10.118.0.4:11023
      route-target both 11023:11023
      redistribute learned
   !
   vlan 1024
      rd 10.118.0.4:11024
      route-target both 11024:11024
      redistribute learned
   !
   vlan 1025
      rd 10.118.0.4:11025
      route-target both 11025:11025
      redistribute learned
   !
   vlan 1026
      rd 10.118.0.4:11026
      route-target both 11026:11026
      redistribute learned
   !
   vlan 1027
      rd 10.118.0.4:11027
      route-target both 11027:11027
      redistribute learned
   !
   vlan 1028
      rd 10.118.0.4:11028
      route-target both 11028:11028
      redistribute learned
   !
   vlan 1029
      rd 10.118.0.4:11029
      route-target both 11029:11029
      redistribute learned
   !
   vlan 1033
      rd 10.118.0.4:11033
      route-target both 11033:11033
      redistribute learned
   !
   vlan 1035
      rd 10.118.0.4:11035
      route-target both 11035:11035
      redistribute learned
   !
   vlan 1036
      rd 10.118.0.4:11036
      route-target both 11036:11036
      redistribute learned
   !
   vlan 1037
      rd 10.118.0.4:11037
      route-target both 11037:11037
      redistribute learned
   !
   vlan 1038
      rd 10.118.0.4:11038
      route-target both 11038:11038
      redistribute learned
   !
   vlan 1050
      rd 10.118.0.4:11050
      route-target both 11050:11050
      redistribute learned
   !
   vlan 1051
      rd 10.118.0.4:11051
      route-target both 11051:11051
      redistribute learned
   !
   vlan 1052
      rd 10.118.0.4:11052
      route-target both 11052:11052
      redistribute learned
   !
   vlan 1053
      rd 10.118.0.4:11053
      route-target both 11053:11053
      redistribute learned
   !
   vlan 1054
      rd 10.118.0.4:11054
      route-target both 11054:11054
      redistribute learned
   !
   vlan 1055
      rd 10.118.0.4:11055
      route-target both 11055:11055
      redistribute learned
   !
   vlan 1056
      rd 10.118.0.4:11056
      route-target both 11056:11056
      redistribute learned
   !
   vlan 1057
      rd 10.118.0.4:11057
      route-target both 11057:11057
      redistribute learned
   !
   vlan 1058
      rd 10.118.0.4:11058
      route-target both 11058:11058
      redistribute learned
   !
   vlan 1060
      rd 10.118.0.4:11060
      route-target both 11060:11060
      redistribute learned
   !
   vlan 1061
      rd 10.118.0.4:11061
      route-target both 11061:11061
      redistribute learned
   !
   vlan 1062
      rd 10.118.0.4:11062
      route-target both 11062:11062
      redistribute learned
   !
   vlan 1101
      rd 10.118.0.4:11101
      route-target both 11101:11101
      redistribute learned
   !
   vlan 1102
      rd 10.118.0.4:11102
      route-target both 11102:11102
      redistribute learned
   !
   vlan 1155
      rd 10.118.0.4:11155
      route-target both 11155:11155
      redistribute learned
   !
   vlan 1157
      rd 10.118.0.4:11157
      route-target both 11157:11157
      redistribute learned
   !
   vlan 1160
      rd 10.118.0.4:11160
      route-target both 11160:11160
      redistribute learned
   !
   vlan 1161
      rd 10.118.0.4:11161
      route-target both 11161:11161
      redistribute learned
   !
   vlan 1162
      rd 10.118.0.4:11162
      route-target both 11162:11162
      redistribute learned
   !
   vlan 1163
      rd 10.118.0.4:11163
      route-target both 11163:11163
      redistribute learned
   !
   vlan 1164
      rd 10.118.0.4:11164
      route-target both 11164:11164
      redistribute learned
   !
   vlan 1165
      rd 10.118.0.4:11165
      route-target both 11165:11165
      redistribute learned
   !
   vlan 1166
      rd 10.118.0.4:11166
      route-target both 11166:11166
      redistribute learned
   !
   vlan 1167
      rd 10.118.0.4:11167
      route-target both 11167:11167
      redistribute learned
   !
   vlan 1168
      rd 10.118.0.4:11168
      route-target both 11168:11168
      redistribute learned
   !
   vlan 1169
      rd 10.118.0.4:11169
      route-target both 11169:11169
      redistribute learned
   !
   vlan 1200
      rd 10.118.0.4:11200
      route-target both 11200:11200
      redistribute learned
   !
   vlan 1201
      rd 10.118.0.4:11201
      route-target both 11201:11201
      redistribute learned
   !
   vlan 1202
      rd 10.118.0.4:11202
      route-target both 11202:11202
      redistribute learned
   !
   vlan 1203
      rd 10.118.0.4:11203
      route-target both 11203:11203
      redistribute learned
   !
   vlan 1204
      rd 10.118.0.4:11204
      route-target both 11204:11204
      redistribute learned
   !
   vlan 1205
      rd 10.118.0.4:11205
      route-target both 11205:11205
      redistribute learned
   !
   vlan 1206
      rd 10.118.0.4:11206
      route-target both 11206:11206
      redistribute learned
   !
   vlan 1301
      rd 10.118.0.4:11301
      route-target both 11301:11301
      redistribute learned
   !
   vlan 1302
      rd 10.118.0.4:11302
      route-target both 11302:11302
      redistribute learned
   !
   vlan 1304
      rd 10.118.0.4:11304
      route-target both 11304:11304
      redistribute learned
   !
   vlan 1309
      rd 10.118.0.4:11309
      route-target both 11309:11309
      redistribute learned
   !
   vlan 1310
      rd 10.118.0.4:11310
      route-target both 11310:11310
      redistribute learned
   !
   vlan 1311
      rd 10.118.0.4:11311
      route-target both 11311:11311
      redistribute learned
   !
   vlan 1350
      rd 10.118.0.4:11350
      route-target both 11350:11350
      redistribute learned
   !
   vlan 1351
      rd 10.118.0.4:11351
      route-target both 11351:11351
      redistribute learned
   !
   vlan 1501
      rd 10.118.0.4:11501
      route-target both 11501:11501
      redistribute learned
   !
   vlan 2000
      rd 10.118.0.4:12000
      route-target both 12000:12000
      redistribute learned
   !
   vlan 2005
      rd 10.118.0.4:12005
      route-target both 12005:12005
      redistribute learned
   !
   vlan 2006
      rd 10.118.0.4:12006
      route-target both 12006:12006
      redistribute learned
   !
   vlan 2007
      rd 10.118.0.4:12007
      route-target both 12007:12007
      redistribute learned
   !
   vlan 2008
      rd 10.118.0.4:12008
      route-target both 12008:12008
      redistribute learned
   !
   vlan 2009
      rd 10.118.0.4:12009
      route-target both 12009:12009
      redistribute learned
   !
   vlan 2010
      rd 10.118.0.4:12010
      route-target both 12010:12010
      redistribute learned
   !
   vlan 2011
      rd 10.118.0.4:12011
      route-target both 12011:12011
      redistribute learned
   !
   vlan 2012
      rd 10.118.0.4:12012
      route-target both 12012:12012
      redistribute learned
   !
   vlan 2013
      rd 10.118.0.4:12013
      route-target both 12013:12013
      redistribute learned
   !
   vlan 2014
      rd 10.118.0.4:12014
      route-target both 12014:12014
      redistribute learned
   !
   vlan 2015
      rd 10.118.0.4:12015
      route-target both 12015:12015
      redistribute learned
   !
   vlan 2016
      rd 10.118.0.4:12016
      route-target both 12016:12016
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf PCS-MPLS-INTRANET
      rd 10.118.0.4:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.118.0.4
      neighbor 10.118.4.250 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.118.4.250 description KDC-AR7508R3-PCS-FELEAF1_Vlan4092
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf PCS-SHARED-INTERNET
      rd 10.118.0.4:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 10.118.0.4
      neighbor 10.118.4.248 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.118.4.248 description KDC-AR7508R3-PCS-FELEAF1_Vlan4091
      redistribute connected route-map RM-CONN-2-BGP-VRFS
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

## Queue Monitor

### Queue Monitor Length

| Enabled | Logging Interval | Default Thresholds High | Default Thresholds Low | Notifying | TX Latency | CPU Thresholds High | CPU Thresholds Low | Mirroring Enabled | Mirror destinations |
| ------- | ---------------- | ----------------------- | ---------------------- | --------- | ---------- | ------------------- | ------------------ | ----------------- | ------------------ |
| True | 30 | - | - | disabled | disabled | - | - | - | - |

### Queue Monitor Streaming

| Enabled | IP Access Group | IPv6 Access Group | Max Connections | VRF |
| ------- | --------------- | ----------------- | --------------- | --- |
| True | - | - | - | - |

### Queue Monitor Configuration

```eos
!
queue-monitor length
!
queue-monitor length log 30
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

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.118.0.0/24 eq 32 |
| 20 | permit 10.118.1.0/25 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.118.4.248/31 |
| 20 | permit 10.118.4.250/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.118.0.0/24 eq 32
   seq 20 permit 10.118.1.0/25 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.118.4.248/31
   seq 20 permit 10.118.4.250/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| PCS-MPLS-INTRANET | enabled |
| PCS-NETINFRA-OOB | disabled |
| PCS-SHARED-INTERNET | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance PCS-MPLS-INTRANET
!
vrf instance PCS-NETINFRA-OOB
!
vrf instance PCS-SHARED-INTERNET
```
