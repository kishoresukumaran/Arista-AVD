# LEAF4

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [Management SSH](#management-ssh)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
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
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
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
| Management1 | oob_management | oob | MGMT | 192.168.0.18/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.0.18/24
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 169.254.169.254 | default | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf default 169.254.169.254
```

### Management SSH

#### SSH Timeout and Management

| Idle Timeout | SSH Management |
| ------------ | -------------- |
| default | Enabled |

#### Max number of SSH sessions limit and per-host limit

| Connection Limit | Max from a single Host |
| ---------------- | ---------------------- |
| - | - |

#### Ciphers and Algorithms

| Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
|---------|----------------------|----------------|---------------------------|
| default | default | default | default |


#### Management SSH Device Configuration

```eos
!
management ssh
   client-alive interval 120
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

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
| admin | - | - | True | - |
| arista | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |
| kishore | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
no username admin
username arista privilege 15 role network-admin nopassword
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
username kishore privilege 15 role network-admin secret sha512 <removed>
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | MGMT | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | - | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| mlag2 | Vlan4094 | 10.255.252.4 | Port-Channel47 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id mlag2
   local-interface Vlan4094
   peer-address 10.255.252.4
   peer-link Port-Channel47
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 16384 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
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
| 10 | DMZ | - |
| 20 | DMZ | - |
| 30 | DMZ | - |
| 3009 | MLAG_iBGP_vrfA | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name DMZ
!
vlan 20
   name DMZ
!
vlan 30
   name DMZ
!
vlan 3009
   name MLAG_iBGP_vrfA
   trunk group LEAF_PEER_L3
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet7 | host2_Ethernet2 | *access | *20 | *- | *- | 7 |
| Ethernet8 | host3_Ethernet2 | *access | *20 | *- | *- | 8 |
| Ethernet47 | MLAG_PEER_LEAF3_Ethernet47 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 47 |
| Ethernet48 | MLAG_PEER_LEAF3_Ethernet48 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 47 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet3 | P2P_LINK_TO_SPINE1_Ethernet6 | routed | - | 192.168.103.25/31 | default | 9214 | False | - | - |
| Ethernet4 | P2P_LINK_TO_SPINE2_Ethernet6 | routed | - | 192.168.103.27/31 | default | 9214 | False | - | - |
| Ethernet5 | P2P_LINK_TO_SPINE3_Ethernet6 | routed | - | 192.168.103.29/31 | default | 9214 | False | - | - |
| Ethernet6 | P2P_LINK_TO_SPINE4_Ethernet6 | routed | - | 192.168.103.31/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet3
   description P2P_LINK_TO_SPINE1_Ethernet6
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.103.25/31
!
interface Ethernet4
   description P2P_LINK_TO_SPINE2_Ethernet6
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.103.27/31
!
interface Ethernet5
   description P2P_LINK_TO_SPINE3_Ethernet6
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.103.29/31
!
interface Ethernet6
   description P2P_LINK_TO_SPINE4_Ethernet6
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.103.31/31
!
interface Ethernet7
   description host2_Ethernet2
   no shutdown
   channel-group 7 mode active
!
interface Ethernet8
   description host3_Ethernet2
   no shutdown
   channel-group 8 mode active
!
interface Ethernet47
   description MLAG_PEER_LEAF3_Ethernet47
   no shutdown
   channel-group 47 mode active
!
interface Ethernet48
   description MLAG_PEER_LEAF3_Ethernet48
   no shutdown
   channel-group 47 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel7 | host2_PortChannel Host2 | switched | access | 20 | - | - | - | - | 7 | - |
| Port-Channel8 | host3_PortChannel Host3 | switched | access | 20 | - | - | - | - | 8 | - |
| Port-Channel47 | MLAG_PEER_LEAF3_Po47 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel7
   description host2_PortChannel Host2
   no shutdown
   switchport
   switchport access vlan 20
   mlag 7
   spanning-tree portfast
!
interface Port-Channel8
   description host3_PortChannel Host3
   no shutdown
   switchport
   switchport access vlan 20
   mlag 8
   spanning-tree portfast
!
interface Port-Channel47
   description MLAG_PEER_LEAF3_Po47
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 192.168.101.4/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 192.168.102.3/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 192.168.101.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 192.168.102.3/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | DMZ | vrfA | - | False |
| Vlan20 | DMZ | vrfA | - | False |
| Vlan30 | DMZ | vrfA | - | False |
| Vlan3009 | MLAG_PEER_L3_iBGP: vrf vrfA | vrfA | 9214 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 9214 | False |
| Vlan4094 | MLAG_PEER | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan10 |  vrfA  |  -  |  10.1.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan20 |  vrfA  |  -  |  10.1.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan30 |  vrfA  |  -  |  10.1.30.1/24  |  -  |  -  |  -  |  -  |
| Vlan3009 |  vrfA  |  10.255.251.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.251.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.252.5/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description DMZ
   no shutdown
   vrf vrfA
   ip address virtual 10.1.10.1/24
!
interface Vlan20
   description DMZ
   no shutdown
   vrf vrfA
   ip address virtual 10.1.20.1/24
!
interface Vlan30
   description DMZ
   no shutdown
   vrf vrfA
   ip address virtual 10.1.30.1/24
!
interface Vlan3009
   description MLAG_PEER_L3_iBGP: vrf vrfA
   no shutdown
   mtu 9214
   vrf vrfA
   ip address 10.255.251.5/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 9214
   ip address 10.255.251.5/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9214
   no autostate
   ip address 10.255.252.5/31
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
| 10 | 10010 | - | - |
| 20 | 10020 | - | - |
| 30 | 10030 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| vrfA | 10 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description LEAF4_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 20 vni 10020
   vxlan vlan 30 vni 10030
   vxlan vrf vrfA vni 10
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
| MGMT | False |
| vrfA | True |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf vrfA
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |
| vrfA | false |

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65299 | 192.168.101.4 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| update wait-install |
| no bgp default ipv4-unicast |
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
| Remote AS | 65299 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.255.251.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 192.168.101.11 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 192.168.101.12 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 192.168.101.13 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 192.168.101.14 | 65001 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 192.168.103.24 | 65001 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.103.26 | 65001 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.103.28 | 65001 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.103.30 | 65001 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.251.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | vrfA | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 192.168.101.4:10010 | 10010:10010 | - | - | learned |
| 20 | 192.168.101.4:10020 | 10020:10020 | - | - | learned |
| 30 | 192.168.101.4:10030 | 10030:10030 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| vrfA | 192.168.101.4:10 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65299
   router-id 192.168.101.4
   maximum-paths 4 ecmp 4
   update wait-install
   no bgp default ipv4-unicast
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65299
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description LEAF3
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.255.251.4 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.251.4 description LEAF3
   neighbor 192.168.101.11 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.101.11 remote-as 65001
   neighbor 192.168.101.11 description SPINE1
   neighbor 192.168.101.12 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.101.12 remote-as 65001
   neighbor 192.168.101.12 description SPINE2
   neighbor 192.168.101.13 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.101.13 remote-as 65001
   neighbor 192.168.101.13 description SPINE3
   neighbor 192.168.101.14 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.101.14 remote-as 65001
   neighbor 192.168.101.14 description SPINE4
   neighbor 192.168.103.24 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.103.24 remote-as 65001
   neighbor 192.168.103.24 description SPINE1_Ethernet6
   neighbor 192.168.103.26 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.103.26 remote-as 65001
   neighbor 192.168.103.26 description SPINE2_Ethernet6
   neighbor 192.168.103.28 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.103.28 remote-as 65001
   neighbor 192.168.103.28 description SPINE3_Ethernet6
   neighbor 192.168.103.30 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.103.30 remote-as 65001
   neighbor 192.168.103.30 description SPINE4_Ethernet6
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 192.168.101.4:10010
      route-target both 10010:10010
      redistribute learned
   !
   vlan 20
      rd 192.168.101.4:10020
      route-target both 10020:10020
      redistribute learned
   !
   vlan 30
      rd 192.168.101.4:10030
      route-target both 10030:10030
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
   vrf vrfA
      rd 192.168.101.4:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 192.168.101.4
      update wait-install
      neighbor 10.255.251.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
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
| 10 | permit 192.168.101.0/24 eq 32 |
| 20 | permit 192.168.102.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.168.101.0/24 eq 32
   seq 20 permit 192.168.102.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

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
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| vrfA | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance vrfA
```
