# LEAF5

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
- [802.1X Port Security](#8021x-port-security)
  - [802.1X Summary](#8021x-summary)
- [Power Over Ethernet (PoE)](#power-over-ethernet-poe)
  - [PoE Summary](#poe-summary)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | MGMT | 172.16.100.105/24 | 172.16.100.1 |

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
   ip address 172.16.100.105/24
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
| Ethernet1 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet2 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet3 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet4 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet5 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet6 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet7 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet8 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet9 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet10 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet11 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet12 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet13 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet14 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet15 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet16 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet17 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet18 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet19 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet20 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet21 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet22 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet23 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet24 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet25 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet26 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet27 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet28 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet29 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet30 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet31 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet32 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet33 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet34 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet35 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet36 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet37 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet38 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet39 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet40 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet41 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet42 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet43 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet44 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet45 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet46 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet47 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet48 | RACK1 Access Port | trunk phone | - | 1 | - | - |
| Ethernet53 | L2_SPINE1_Ethernet23 | *trunk | *10,20,30,50,60,100 | *- | *- | 53 |
| Ethernet54 | L2_SPINE2_Ethernet23 | *trunk | *10,20,30,50,60,100 | *- | *- | 53 |

*Inherited from Port-Channel Interface

##### Phone Interfaces

| Interface | Mode | Native VLAN | Phone VLAN | Phone VLAN Mode |
| --------- | ---- | ----------- | ---------- | --------------- |
| Ethernet1 | trunk phone | 1 | 40 | untagged |
| Ethernet2 | trunk phone | 1 | 40 | untagged |
| Ethernet3 | trunk phone | 1 | 40 | untagged |
| Ethernet4 | trunk phone | 1 | 40 | untagged |
| Ethernet5 | trunk phone | 1 | 40 | untagged |
| Ethernet6 | trunk phone | 1 | 40 | untagged |
| Ethernet7 | trunk phone | 1 | 40 | untagged |
| Ethernet8 | trunk phone | 1 | 40 | untagged |
| Ethernet9 | trunk phone | 1 | 40 | untagged |
| Ethernet10 | trunk phone | 1 | 40 | untagged |
| Ethernet11 | trunk phone | 1 | 40 | untagged |
| Ethernet12 | trunk phone | 1 | 40 | untagged |
| Ethernet13 | trunk phone | 1 | 40 | untagged |
| Ethernet14 | trunk phone | 1 | 40 | untagged |
| Ethernet15 | trunk phone | 1 | 40 | untagged |
| Ethernet16 | trunk phone | 1 | 40 | untagged |
| Ethernet17 | trunk phone | 1 | 40 | untagged |
| Ethernet18 | trunk phone | 1 | 40 | untagged |
| Ethernet19 | trunk phone | 1 | 40 | untagged |
| Ethernet20 | trunk phone | 1 | 40 | untagged |
| Ethernet21 | trunk phone | 1 | 40 | untagged |
| Ethernet22 | trunk phone | 1 | 40 | untagged |
| Ethernet23 | trunk phone | 1 | 40 | untagged |
| Ethernet24 | trunk phone | 1 | 40 | untagged |
| Ethernet25 | trunk phone | 1 | 40 | untagged |
| Ethernet26 | trunk phone | 1 | 40 | untagged |
| Ethernet27 | trunk phone | 1 | 40 | untagged |
| Ethernet28 | trunk phone | 1 | 40 | untagged |
| Ethernet29 | trunk phone | 1 | 40 | untagged |
| Ethernet30 | trunk phone | 1 | 40 | untagged |
| Ethernet31 | trunk phone | 1 | 40 | untagged |
| Ethernet32 | trunk phone | 1 | 40 | untagged |
| Ethernet33 | trunk phone | 1 | 40 | untagged |
| Ethernet34 | trunk phone | 1 | 40 | untagged |
| Ethernet35 | trunk phone | 1 | 40 | untagged |
| Ethernet36 | trunk phone | 1 | 40 | untagged |
| Ethernet37 | trunk phone | 1 | 40 | untagged |
| Ethernet38 | trunk phone | 1 | 40 | untagged |
| Ethernet39 | trunk phone | 1 | 40 | untagged |
| Ethernet40 | trunk phone | 1 | 40 | untagged |
| Ethernet41 | trunk phone | 1 | 40 | untagged |
| Ethernet42 | trunk phone | 1 | 40 | untagged |
| Ethernet43 | trunk phone | 1 | 40 | untagged |
| Ethernet44 | trunk phone | 1 | 40 | untagged |
| Ethernet45 | trunk phone | 1 | 40 | untagged |
| Ethernet46 | trunk phone | 1 | 40 | untagged |
| Ethernet47 | trunk phone | 1 | 40 | untagged |
| Ethernet48 | trunk phone | 1 | 40 | untagged |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet2
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet3
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet4
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet5
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet6
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet7
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet8
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet9
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet10
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet11
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet12
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet13
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet14
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet15
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet16
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet17
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet18
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet19
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet20
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet21
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet22
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet23
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet24
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet25
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet26
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet27
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet28
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet29
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet30
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet31
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet32
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet33
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet34
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet35
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet36
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet37
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet38
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet39
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet40
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet41
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet42
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet43
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet44
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet45
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet46
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet47
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet48
   description RACK1 Access Port
   no shutdown
   switchport trunk native vlan 1
   switchport phone vlan 40
   switchport phone trunk untagged
   switchport mode trunk phone
   switchport
   poe priority critical
   poe reboot action maintain
   poe link down action maintain
   poe shutdown action power-off
   poe limit 30.00 watts
   spanning-tree portfast
   spanning-tree bpduguard enable
   dot1x pae authenticator
   dot1x authentication failure action traffic allow vlan 90
   dot1x reauthentication
   dot1x port-control auto
   dot1x host-mode multi-host authenticated
   dot1x mac based authentication
   dot1x timeout tx-period 3
   dot1x timeout reauth-period server
   dot1x reauthorization request limit 3
!
interface Ethernet53
   description L2_SPINE1_Ethernet23
   no shutdown
   channel-group 53 mode active
!
interface Ethernet54
   description L2_SPINE2_Ethernet23
   no shutdown
   channel-group 53 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel53 | L2_SPINES_Port-Channel23 | trunk | 10,20,30,50,60,100 | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel53
   description L2_SPINES_Port-Channel23
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

## 802.1X Port Security

### 802.1X Summary

#### 802.1X Interfaces

| Interface | PAE Mode | State | Phone Force Authorized | Reauthentication | Auth Failure Action | Host Mode | Mac Based Auth | Eapol |
| --------- | -------- | ------| ---------------------- | ---------------- | ------------------- | --------- | -------------- | ------ |
| Ethernet1 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet2 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet3 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet4 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet5 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet6 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet7 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet8 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet9 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet10 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet11 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet12 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet13 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet14 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet15 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet16 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet17 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet18 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet19 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet20 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet21 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet22 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet23 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet24 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet25 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet26 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet27 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet28 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet29 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet30 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet31 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet32 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet33 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet34 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet35 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet36 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet37 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet38 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet39 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet40 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet41 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet42 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet43 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet44 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet45 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet46 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet47 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |
| Ethernet48 | authenticator | auto | - | True | allow vlan 90 | multi-host | True | - |

## Power Over Ethernet (PoE)

### PoE Summary

#### PoE Interfaces

| Interface | PoE Enabled | Priority | Limit | Reboot Action | Link Down Action | Shutdown Action | LLDP Negotiation | Legacy Detection |
| --------- | --------- | --------- | ----------- | ----------- | ----------- | ----------- | --------- | --------- |
| Ethernet1 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet2 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet3 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet4 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet5 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet6 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet7 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet8 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet9 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet10 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet11 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet12 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet13 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet14 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet15 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet16 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet17 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet18 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet19 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet20 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet21 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet22 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet23 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet24 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet25 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet26 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet27 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet28 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet29 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet30 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet31 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet32 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet33 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet34 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet35 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet36 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet37 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet38 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet39 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet40 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet41 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet42 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet43 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet44 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet45 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet46 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet47 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |
| Ethernet48 | True | critical | 30.00 watts | maintain | maintain | power-off | - | - |

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
