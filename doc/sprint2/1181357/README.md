# RCOMP 2021-2022 Project - Sprint 2 - Building 3 - 1181357
===========================================

## Analysing Steps for Building 3

* [General Information](#general-information) 
* [Layer 2 Configuration](#layer-2-configuration) 
* [Layer 3 Configuration](#layer-3-configuration) 


---
## General Information <a name="general-information"></a>
* Packet Tracer Version 
    8.1.1.0022
* Device naming follows the naming convention defined on the planning document.

## Layer 2 Configuration <a name="layer-2-configuration"></a> 
* The orange section represents the DMZ VLAN.
* The green section represents the VIP VLAN.
* The yellow section represents the Floor 0 VLAN.
* The light blue section represents the Floor 1 VLAN.
* The blue section represents the WIFI VLAN.
* The cable type will also match the ones settled in the previous sprint. Fiber between Intermediate cross-connects and copper for everything else.

* Redundant cable link are also used to connect the horizontal cross-connects and the intermediate cross-connects. Using the ring redundance.


### Virtual Lans

Taking from the planning file, the Vlan's name and id are the following:


| Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |   Backbone  |
| :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: |
| 3        | B3F0 - 505  | B3F1 - 506  | B3WF - 507  | B3DMZ - 508 | B3VIP - 509 | B1BBC - 520 |


### VLAN VTP configuration

VTP server name used : **rc22nag2**

* VTP Server: B3F0_IC(Intermediate cross-connect)
* VTP Clients: B3F0_HC, B3F0_CP1, B3F0_CP2,  B3F0_CP3, B3F0_CP4, B3F1_HC, B3F1_CP1, B3F1_CP2,  B3F1_CP3, B3F1_CP4


## Layer 3 Configuration <a name="layer-3-configuration"></a>

### IPv4 Configuration

Taking from the planning file, the IPv4 configurations is the following:


  | Building | Hosts Needed | Hosts Available | Subnet Address  | Mask          |      
  | :------: | :---------:  | :-------------: | :-------------: | :-----------: | 
  | 3      | 188          | 254             | 172.18.187.0/24 | 255.255.255.0	|
  |Backbone  | 120          | 512             | 172.18.190.0/23 | 255.255.254.0	|

### Building 3 IPv4 Configuration
 **172.18.187.0/24**

 The following table has the network IPs for each VLAN needed for Building 3:

  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B3WF  | 55          | 172.18.187.0/26   | 255.255.255.192 | 62            |
  | B3F1  | 45           | 172.18.187.64/26 | 255.255.255.192 | 62              |
  | B3F0  | 35           | 172.18.187.128/26 | 255.255.255.192 | 62              |
  | B3DMZ | 28           | 172.18.187.192/27 | 255.255.255.224 | 30              |
  | B3VIP | 25           | 172.18.187.224/27 | 255.255.255.224 | 30              |


## Router and Static Routing

* The router redirects all external traffic to the MC router *172.18.190.1*
* DHCP pools where used as it allows for dynamic attribution of IPs within the network.
