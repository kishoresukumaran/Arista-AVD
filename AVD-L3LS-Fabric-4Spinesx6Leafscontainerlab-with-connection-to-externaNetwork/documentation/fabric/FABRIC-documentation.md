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
| FABRIC | l3leaf | LEAF1 | 192.168.0.15/24 | - | Provisioned | - |
| FABRIC | l3leaf | LEAF2 | 192.168.0.16/24 | - | Provisioned | - |
| FABRIC | l3leaf | LEAF3 | 192.168.0.17/24 | - | Provisioned | - |
| FABRIC | l3leaf | LEAF4 | 192.168.0.18/24 | - | Provisioned | - |
| FABRIC | l3leaf | LEAF5 | 192.168.0.19/24 | - | Provisioned | - |
| FABRIC | l3leaf | LEAF6 | 192.168.0.20/24 | - | Provisioned | - |
| FABRIC | spine | SPINE1 | 192.168.0.11/24 | - | Provisioned | - |
| FABRIC | spine | SPINE2 | 192.168.0.12/24 | - | Provisioned | - |
| FABRIC | spine | SPINE3 | 192.168.0.13/24 | - | Provisioned | - |
| FABRIC | spine | SPINE4 | 192.168.0.14/24 | - | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | LEAF1 | Ethernet3 | spine | SPINE1 | Ethernet3 |
| l3leaf | LEAF1 | Ethernet4 | spine | SPINE2 | Ethernet3 |
| l3leaf | LEAF1 | Ethernet5 | spine | SPINE3 | Ethernet3 |
| l3leaf | LEAF1 | Ethernet6 | spine | SPINE4 | Ethernet3 |
| l3leaf | LEAF1 | Ethernet47 | mlag_peer | LEAF2 | Ethernet47 |
| l3leaf | LEAF1 | Ethernet48 | mlag_peer | LEAF2 | Ethernet48 |
| l3leaf | LEAF2 | Ethernet3 | spine | SPINE1 | Ethernet4 |
| l3leaf | LEAF2 | Ethernet4 | spine | SPINE2 | Ethernet4 |
| l3leaf | LEAF2 | Ethernet5 | spine | SPINE3 | Ethernet4 |
| l3leaf | LEAF2 | Ethernet6 | spine | SPINE4 | Ethernet4 |
| l3leaf | LEAF3 | Ethernet3 | spine | SPINE1 | Ethernet5 |
| l3leaf | LEAF3 | Ethernet4 | spine | SPINE2 | Ethernet5 |
| l3leaf | LEAF3 | Ethernet5 | spine | SPINE3 | Ethernet5 |
| l3leaf | LEAF3 | Ethernet6 | spine | SPINE4 | Ethernet5 |
| l3leaf | LEAF3 | Ethernet47 | mlag_peer | LEAF4 | Ethernet47 |
| l3leaf | LEAF3 | Ethernet48 | mlag_peer | LEAF4 | Ethernet48 |
| l3leaf | LEAF4 | Ethernet3 | spine | SPINE1 | Ethernet6 |
| l3leaf | LEAF4 | Ethernet4 | spine | SPINE2 | Ethernet6 |
| l3leaf | LEAF4 | Ethernet5 | spine | SPINE3 | Ethernet6 |
| l3leaf | LEAF4 | Ethernet6 | spine | SPINE4 | Ethernet6 |
| l3leaf | LEAF5 | Ethernet3 | spine | SPINE1 | Ethernet7 |
| l3leaf | LEAF5 | Ethernet4 | spine | SPINE2 | Ethernet7 |
| l3leaf | LEAF5 | Ethernet5 | spine | SPINE3 | Ethernet7 |
| l3leaf | LEAF5 | Ethernet6 | spine | SPINE4 | Ethernet7 |
| l3leaf | LEAF5 | Ethernet47 | mlag_peer | LEAF6 | Ethernet47 |
| l3leaf | LEAF5 | Ethernet48 | mlag_peer | LEAF6 | Ethernet48 |
| l3leaf | LEAF6 | Ethernet3 | spine | SPINE1 | Ethernet8 |
| l3leaf | LEAF6 | Ethernet4 | spine | SPINE2 | Ethernet8 |
| l3leaf | LEAF6 | Ethernet5 | spine | SPINE3 | Ethernet8 |
| l3leaf | LEAF6 | Ethernet6 | spine | SPINE4 | Ethernet8 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 192.168.103.0/24 | 256 | 48 | 18.75 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| LEAF1 | Ethernet3 | 192.168.103.1/31 | SPINE1 | Ethernet3 | 192.168.103.0/31 |
| LEAF1 | Ethernet4 | 192.168.103.3/31 | SPINE2 | Ethernet3 | 192.168.103.2/31 |
| LEAF1 | Ethernet5 | 192.168.103.5/31 | SPINE3 | Ethernet3 | 192.168.103.4/31 |
| LEAF1 | Ethernet6 | 192.168.103.7/31 | SPINE4 | Ethernet3 | 192.168.103.6/31 |
| LEAF2 | Ethernet3 | 192.168.103.9/31 | SPINE1 | Ethernet4 | 192.168.103.8/31 |
| LEAF2 | Ethernet4 | 192.168.103.11/31 | SPINE2 | Ethernet4 | 192.168.103.10/31 |
| LEAF2 | Ethernet5 | 192.168.103.13/31 | SPINE3 | Ethernet4 | 192.168.103.12/31 |
| LEAF2 | Ethernet6 | 192.168.103.15/31 | SPINE4 | Ethernet4 | 192.168.103.14/31 |
| LEAF3 | Ethernet3 | 192.168.103.17/31 | SPINE1 | Ethernet5 | 192.168.103.16/31 |
| LEAF3 | Ethernet4 | 192.168.103.19/31 | SPINE2 | Ethernet5 | 192.168.103.18/31 |
| LEAF3 | Ethernet5 | 192.168.103.21/31 | SPINE3 | Ethernet5 | 192.168.103.20/31 |
| LEAF3 | Ethernet6 | 192.168.103.23/31 | SPINE4 | Ethernet5 | 192.168.103.22/31 |
| LEAF4 | Ethernet3 | 192.168.103.25/31 | SPINE1 | Ethernet6 | 192.168.103.24/31 |
| LEAF4 | Ethernet4 | 192.168.103.27/31 | SPINE2 | Ethernet6 | 192.168.103.26/31 |
| LEAF4 | Ethernet5 | 192.168.103.29/31 | SPINE3 | Ethernet6 | 192.168.103.28/31 |
| LEAF4 | Ethernet6 | 192.168.103.31/31 | SPINE4 | Ethernet6 | 192.168.103.30/31 |
| LEAF5 | Ethernet3 | 192.168.103.33/31 | SPINE1 | Ethernet7 | 192.168.103.32/31 |
| LEAF5 | Ethernet4 | 192.168.103.35/31 | SPINE2 | Ethernet7 | 192.168.103.34/31 |
| LEAF5 | Ethernet5 | 192.168.103.37/31 | SPINE3 | Ethernet7 | 192.168.103.36/31 |
| LEAF5 | Ethernet6 | 192.168.103.39/31 | SPINE4 | Ethernet7 | 192.168.103.38/31 |
| LEAF6 | Ethernet3 | 192.168.103.41/31 | SPINE1 | Ethernet8 | 192.168.103.40/31 |
| LEAF6 | Ethernet4 | 192.168.103.43/31 | SPINE2 | Ethernet8 | 192.168.103.42/31 |
| LEAF6 | Ethernet5 | 192.168.103.45/31 | SPINE3 | Ethernet8 | 192.168.103.44/31 |
| LEAF6 | Ethernet6 | 192.168.103.47/31 | SPINE4 | Ethernet8 | 192.168.103.46/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 192.168.101.0/24 | 256 | 10 | 3.91 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | LEAF1 | 192.168.101.1/32 |
| FABRIC | LEAF2 | 192.168.101.2/32 |
| FABRIC | LEAF3 | 192.168.101.3/32 |
| FABRIC | LEAF4 | 192.168.101.4/32 |
| FABRIC | LEAF5 | 192.168.101.5/32 |
| FABRIC | LEAF6 | 192.168.101.6/32 |
| FABRIC | SPINE1 | 192.168.101.11/32 |
| FABRIC | SPINE2 | 192.168.101.12/32 |
| FABRIC | SPINE3 | 192.168.101.13/32 |
| FABRIC | SPINE4 | 192.168.101.14/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 192.168.102.0/24 | 256 | 6 | 2.35 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | LEAF1 | 192.168.102.1/32 |
| FABRIC | LEAF2 | 192.168.102.1/32 |
| FABRIC | LEAF3 | 192.168.102.3/32 |
| FABRIC | LEAF4 | 192.168.102.3/32 |
| FABRIC | LEAF5 | 192.168.102.5/32 |
| FABRIC | LEAF6 | 192.168.102.5/32 |
