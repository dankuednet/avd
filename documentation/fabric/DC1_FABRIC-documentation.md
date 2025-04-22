# DC1_FABRIC

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
- [Connected Endpoints](#connected-endpoints)
  - [Connected Endpoint Keys](#connected-endpoint-keys)
  - [Firewalls](#firewalls)
  - [Printers](#printers)
  - [Port Profiles](#port-profiles)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| DC1_FABRIC | l2leaf | LEAF1 | 172.16.100.101/24 | 7050X3 | Provisioned | - |
| DC1_FABRIC | l2leaf | LEAF2 | 172.16.100.102/24 | 7050X3 | Provisioned | - |
| DC1_FABRIC | l2leaf | LEAF3 | 172.16.100.103/24 | 720XP | Provisioned | - |
| DC1_FABRIC | l2leaf | LEAF4 | 172.16.100.104/24 | 720XP | Provisioned | - |
| DC1_FABRIC | l2leaf | LEAF5 | 172.16.100.105/24 | 720XP | Provisioned | - |
| DC1_FABRIC | l2spine | SPINE1 | 172.16.100.11/24 | 7050X3 | Provisioned | - |
| DC1_FABRIC | l2spine | SPINE2 | 172.16.100.12/24 | 7050X3 | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l2leaf | LEAF1 | Ethernet35 | l2spine | SPINE1 | Ethernet31 |
| l2leaf | LEAF1 | Ethernet36 | l2spine | SPINE2 | Ethernet31 |
| l2leaf | LEAF2 | Ethernet35 | l2spine | SPINE1 | Ethernet29 |
| l2leaf | LEAF2 | Ethernet36 | l2spine | SPINE2 | Ethernet29 |
| l2leaf | LEAF3 | Ethernet53 | l2spine | SPINE1 | Ethernet27 |
| l2leaf | LEAF3 | Ethernet54 | l2spine | SPINE2 | Ethernet27 |
| l2leaf | LEAF4 | Ethernet53 | l2spine | SPINE1 | Ethernet25 |
| l2leaf | LEAF4 | Ethernet54 | l2spine | SPINE2 | Ethernet25 |
| l2leaf | LEAF5 | Ethernet53 | l2spine | SPINE1 | Ethernet23 |
| l2leaf | LEAF5 | Ethernet54 | l2spine | SPINE2 | Ethernet23 |
| l2spine | SPINE1 | Ethernet32 | mlag_peer | SPINE2 | Ethernet32 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |

## Connected Endpoints

### Connected Endpoint Keys

| Key | Type | Description |
| --- | ---- | ----------- |
| firewalls | firewall | - |
| printers | printers | - |

### Firewalls

| Name | Port | Fabric Device | Fabric Port | Description | Shutdown | Mode | Access VLAN | Trunk Allowed VLANs | Profile |
| ---- | ---- | ------------- | ------------| ----------- | -------- | ---- | ----------- | ------------------- | ------- |
| FIREWALL | Eth1 | SPINE1 | Ethernet5 | FIREWALL_FIREWALL_Eth1 | False | trunk | - | 20,30,40,50,60,80,110 | PP-FIREWALL |
| FIREWALL | Eth2 | SPINE2 | Ethernet5 | FIREWALL_FIREWALL_Eth2 | False | trunk | - | 20,30,40,50,60,80,110 | PP-FIREWALL |

### Printers

| Name | Port | Fabric Device | Fabric Port | Description | Shutdown | Mode | Access VLAN | Trunk Allowed VLANs | Profile |
| ---- | ---- | ------------- | ------------| ----------- | -------- | ---- | ----------- | ------------------- | ------- |
| PRN2 | Eth1 | LEAF4 | Ethernet5 | PRINTERS_PRN2_Eth1 | False | access | 30 | - | PP-PRN |

### Port Profiles

| Profile Name | Parent Profile |
| ------------ | -------------- |
| PP-AP | PP-DEFAULTS |
| PP-BLACKHOLE | PP-DEFAULTS |
| PP-DATA | PP-DEFAULTS |
| PP-DEFAULTS | - |
| PP-DOT1X | - |
| PP-FIREWALL | - |
| PP-GUEST | PP-DEFAULTS |
| PP-MNG | PP-DEFAULTS |
| PP-PRN | PP-DEFAULTS |
| PP-SVR | PP-DEFAULTS |
| PP-VIDEO | PP-DEFAULTS |
| PP-VLM | PP-DEFAULTS |
| PP-VOIP | PP-DEFAULTS |
| PP-WS | PP-DEFAULTS |
