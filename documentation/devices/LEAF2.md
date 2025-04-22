# LEAF2

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
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
| Management1 | OOB_MANAGEMENT | oob | MGMT | 172.16.100.102/24 | 172.16.100.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   vrf MGMT
   ip address 172.16.100.102/24
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 192.168.2.249 | MGMT | - |
| 192.168.2.248 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 192.168.2.248
ip name-server vrf MGMT 192.168.2.249
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| ntp.ued.net | MGMT | True | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT ntp.ued.net prefer
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
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
username admin privilege 15 role network-admin secret sha512 <removed>
username arista privilege 15 role network-admin nopassword
```

### Enable Password

Enable password has been disabled

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |

Policy local allow-nopassword-remote-login has been enabled.

#### AAA Authentication Device Configuration

```eos
aaa authentication policy local allow-nopassword-remote-login
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 16384 |

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
spanning-tree mst 0 priority 16384
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
| 10 | DATA-NET | - |
| 20 | PRN-NET | - |
| 30 | VOIP-NET | - |
| 50 | MNG-NET | - |
| 60 | GUEST-NET | - |
| 100 | BLACKHOLE-NET | - |

### VLANs Device Configuration

```eos
!
vlan 10
   name DATA-NET
!
vlan 20
   name PRN-NET
!
vlan 30
   name VOIP-NET
!
vlan 50
   name MNG-NET
!
vlan 60
   name GUEST-NET
!
vlan 100
   name BLACKHOLE-NET
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet2 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet3 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet4 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet5 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet6 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet7 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet8 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet9 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet10 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet11 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet12 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet13 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet14 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet15 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet16 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet17 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet18 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet19 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet20 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet21 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet22 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet23 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet24 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet25 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet26 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet27 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet28 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet29 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet30 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet31 | RACK1 ToR Port | trunk | 1,20,40,110 | - | - | - |
| Ethernet32 | RACK1 ToR Port | - | - | - | - | - |
| Ethernet35 | L2_SPINE1_Ethernet29 | *trunk | *10,20,30,50,60,100 | *- | *- | 35 |
| Ethernet36 | L2_SPINE2_Ethernet29 | *trunk | *10,20,30,50,60,100 | *- | *- | 35 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet2
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet3
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet4
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet5
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet6
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet7
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet8
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet9
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet10
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet11
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet12
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet13
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet14
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet15
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet16
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet17
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet18
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet19
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet20
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet21
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet22
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet23
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet24
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet25
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet26
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet27
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet28
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet29
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet30
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet31
   description RACK1 ToR Port
   no shutdown
   switchport trunk allowed vlan 1,20,40,110
   switchport mode trunk
   switchport
   spanning-tree portfast
!
interface Ethernet32
   description RACK1 ToR Port
   shutdown
   switchport
!
interface Ethernet35
   description L2_SPINE1_Ethernet29
   no shutdown
   channel-group 35 mode active
!
interface Ethernet36
   description L2_SPINE2_Ethernet29
   no shutdown
   channel-group 35 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel35 | L2_SPINES_Port-Channel29 | trunk | 10,20,30,50,60,100 | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel35
   description L2_SPINES_Port-Channel29
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
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
| MGMT | False |

#### IP Routing Device Configuration

```eos
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 172.16.100.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 172.16.100.1
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
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```
