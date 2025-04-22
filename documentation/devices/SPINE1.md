# SPINE1

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
  - [VLAN Interfaces](#vlan-interfaces)
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
| Management1 | OOB_MANAGEMENT | oob | MGMT | 172.16.100.11/24 | 172.16.100.1 |

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
   ip address 172.16.100.11/24
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

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| SPINES | Vlan4094 | 192.168.0.1 | Port-Channel31 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id SPINES
   local-interface Vlan4094
   peer-address 192.168.0.1
   peer-link Port-Channel31
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

- Spanning Tree disabled for VLANs: **4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4094
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
| 10 | DATA-NET | - |
| 20 | PRN-NET | - |
| 30 | VOIP-NET | - |
| 50 | MNG-NET | - |
| 60 | GUEST-NET | - |
| 100 | BLACKHOLE-NET | - |
| 4094 | MLAG | MLAG |

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
| Ethernet5 | FIREWALL_FIREWALL_Eth1 | *trunk | *20,30,40,50,60,80,110 | *- | *- | 5 |
| Ethernet23 | L2_LEAF5_Ethernet53 | *trunk | *10,20,30,50,60,100 | *- | *- | 23 |
| Ethernet25 | L2_LEAF4_Ethernet53 | *trunk | *10,20,30,50,60,100 | *- | *- | 25 |
| Ethernet27 | L2_LEAF3_Ethernet53 | *trunk | *10,20,30,50,60,100 | *- | *- | 27 |
| Ethernet29 | L2_LEAF2_Ethernet35 | *trunk | *10,20,30,50,60,100 | *- | *- | 29 |
| Ethernet31 | L2_LEAF1_Ethernet35 | *trunk | *10,20,30,50,60,100 | *- | *MLAG | 31 |
| Ethernet32 | MLAG_SPINE2_Ethernet32 | *trunk | *10,20,30,50,60,100 | *- | *MLAG | 31 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet5
   description FIREWALL_FIREWALL_Eth1
   no shutdown
   channel-group 5 mode active
!
interface Ethernet23
   description L2_LEAF5_Ethernet53
   no shutdown
   channel-group 23 mode active
!
interface Ethernet25
   description L2_LEAF4_Ethernet53
   no shutdown
   channel-group 25 mode active
!
interface Ethernet27
   description L2_LEAF3_Ethernet53
   no shutdown
   channel-group 27 mode active
!
interface Ethernet29
   description L2_LEAF2_Ethernet35
   no shutdown
   channel-group 29 mode active
!
interface Ethernet31
   description L2_LEAF1_Ethernet35
   no shutdown
   channel-group 31 mode active
!
interface Ethernet32
   description MLAG_SPINE2_Ethernet32
   no shutdown
   channel-group 31 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel5 | FIREWALL_FIREWALL | trunk | 20,30,40,50,60,80,110 | - | - | - | - | 5 | - |
| Port-Channel23 | L2_LEAF5_Port-Channel53 | trunk | 10,20,30,50,60,100 | - | - | - | - | 23 | - |
| Port-Channel25 | L2_LEAF4_Port-Channel53 | trunk | 10,20,30,50,60,100 | - | - | - | - | 25 | - |
| Port-Channel27 | L2_LEAF3_Port-Channel53 | trunk | 10,20,30,50,60,100 | - | - | - | - | 27 | - |
| Port-Channel29 | L2_LEAF2_Port-Channel35 | trunk | 10,20,30,50,60,100 | - | - | - | - | 29 | - |
| Port-Channel31 | L2_LEAF1_Port-Channel35 | trunk | 10,20,30,50,60,100 | - | MLAG | - | - | 31 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel5
   description FIREWALL_FIREWALL
   no shutdown
   switchport trunk allowed vlan 20,30,40,50,60,80,110
   switchport mode trunk
   switchport
   mlag 5
!
interface Port-Channel23
   description L2_LEAF5_Port-Channel53
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
   switchport mode trunk
   switchport
   mlag 23
!
interface Port-Channel25
   description L2_LEAF4_Port-Channel53
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
   switchport mode trunk
   switchport
   mlag 25
!
interface Port-Channel27
   description L2_LEAF3_Port-Channel53
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
   switchport mode trunk
   switchport
   mlag 27
!
interface Port-Channel29
   description L2_LEAF2_Port-Channel35
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
   switchport mode trunk
   switchport
   mlag 29
!
interface Port-Channel31
   description L2_LEAF1_Port-Channel35
   no shutdown
   switchport trunk allowed vlan 10,20,30,50,60,100
   switchport mode trunk
   switchport trunk group MLAG
   switchport
   mlag 31
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan4094 |  default  |  192.168.0.0/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 192.168.0.0/31
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
