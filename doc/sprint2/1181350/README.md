# RCOMP 2021-2022 Project - Sprint 2 - Building 2 - 1181350
===========================================

## Analysing Steps for Building 2

* [General Information](#general-information) 
* [Layer 2 Configuration](#layer-2-configuration) 
* [Layer 3 Configuration](#layer-3-configuration) 


---
## General Information <a name="general-information"></a>
* Packet Tracer Version 
    8.1.1.0022
* Device naming
    The devices naming follows the planning naming convention.

## Layer 2 Configuration <a name="layer-2-configuration"></a>
* The section blue on the left corresponds to the floor 0 *(B2F0_HC, B2F0_CP1, B2F0_CP2, B2F0_AP)* of the building, and the green on right the floor 1 *(B2F1_HC, B2F1_CP1, B2F1_CP2, B2F1_CP3, B2F1_AP)*.

* The cable type will also match the ones settled in the previous sprint. Fiber between Intermediate cross-connects and Copper for everything else.

* Redundant cable link are also used to connect the horizontal cross-connects and the intermediate cross-connects. Using the ring redundance.


### Virtual Lans

Taking from the planning file, the Vlan's name and id are the following:


| Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |   Backbone  |
| :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: |
| 2        | B2F0 - 500  | B2F1 - 501  | B2WF - 502  | B2DMZ - 503 | B2VIP - 504 | B1BBC - 520 |


### VLAN VTP configuration

VTP server name used : **rc22nag2**

It is set in every switch of the building where:
  * B2F0_ICR (router representing Intermediate cross-connect) is in server mode
  * B2F0_IC, B2F0_HC, B2F1_HC, B2F0_CP1, B2F0_CP2, B2F1_CP1, B2F1_CP2, B2F1_CP3 are in client mode


## Layer 3 Configuration <a name="layer-3-configuration"></a>

### IPv4 Configuration

Taking from the planning file, the IPv4 configurations is the following:


  | Building | Hosts Needed | Hosts Available | Subnet Address  | Mask          |      
  | :------: | :---------:  | :-------------: | :-------------: | :-----------: | 
  | 2        | 219          | 254             | 172.18.186.0/24 | 255.255.255.0	|
  |Backbone  | 120          | 512             | 172.18.190.0/23 | 255.255.254.0	|

### Building 5 IPv4 Configuration
 **172.18.186.0/24**

 The following table has the network IPs for each VLAN needed for Building 2:

  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B2WF  | 120          | 172.18.186.0/25   | 255.255.255.128 | 126             |
  | B2F1  | 50           | 172.18.186.128/26 | 255.255.255.192 | 62              |
  | B2F0  | 25           | 172.18.186.192/27 | 255.255.255.224 | 30              |
  | B2DMZ | 12           | 172.18.186.224/28 | 255.255.255.240 | 14              |
  | B2VIP | 12           | 172.18.186.240/28 | 255.255.255.240 | 14              |


## Router and Static Routing

* The router redirects all external traffic to the MC router *172.18.190.1*
* DHCP pools where used as it allows for dynamic attribution of IPs within the network.
