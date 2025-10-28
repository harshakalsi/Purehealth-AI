# dc1-leaf1c

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
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
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
| Management1 | OOB_MANAGEMENT | oob | PCS-NETINFRA-OOB | 10.118.5.18/24 | 10.118.5.254 |

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
   ip address 10.118.5.18/24
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
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | L2_KDC-AR7508R3-PCS-FELEAF1_Ethernet8 | *trunk | *5,7-8,14,17-18,303,1000,1004,1006,1009-1029,1033,1035-1038,1050-1058,1060-1062,1101-1102,1155,1157,1160-1169,1200-1206,1301-1302,1304,1309-1311,1350-1351,1501,2000,2005-2016 | *- | *- | 1 |
| Ethernet2 | L2_KDC-AR7508R3-PCS-FELEAF2_Ethernet8 | *trunk | *5,7-8,14,17-18,303,1000,1004,1006,1009-1029,1033,1035-1038,1050-1058,1060-1062,1101-1102,1155,1157,1160-1169,1200-1206,1301-1302,1304,1309-1311,1350-1351,1501,2000,2005-2016 | *- | *- | 1 |
| Ethernet5 | SERVER_dc1-leaf1-server1_iLO | access | 11 | - | - | - |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description L2_KDC-AR7508R3-PCS-FELEAF1_Ethernet8
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description L2_KDC-AR7508R3-PCS-FELEAF2_Ethernet8
   no shutdown
   channel-group 1 mode active
!
interface Ethernet5
   description SERVER_dc1-leaf1-server1_iLO
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
| Port-Channel1 | L2_FE_LEAF_Port-Channel8 | trunk | 5,7-8,14,17-18,303,1000,1004,1006,1009-1029,1033,1035-1038,1050-1058,1060-1062,1101-1102,1155,1157,1160-1169,1200-1206,1301-1302,1304,1309-1311,1350-1351,1501,2000,2005-2016 | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description L2_FE_LEAF_Port-Channel8
   no shutdown
   switchport trunk allowed vlan 5,7-8,14,17-18,303,1000,1004,1006,1009-1029,1033,1035-1038,1050-1058,1060-1062,1101-1102,1155,1157,1160-1169,1200-1206,1301-1302,1304,1309-1311,1350-1351,1501,2000,2005-2016
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
