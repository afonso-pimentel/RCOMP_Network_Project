# RCOMP 2021-2022 Project - Sprint 2 - Building 5 - 1181654
===========================================


## Analysing Steps for Building 5

* [General Information](#general-information) 
* [Layer 2 Configuration](#layer-2-configuration) 
* [Layer 3 Configuration](#layer-3-configuration) 


---
## General Information <a name="general-information"></a>
* Packet Tracer Version 
    8.1.1.0022
* Device naming
    Devices naming convention:

    * Main cross connect : "MC"
    * Intermidiate cross-connect : "ICB" + [building number] ex: ICB5
    * Horizontal cross-connect : "HCB" + [building number] ex: HCB5
    * Consolidation point : "CP" + [room number without (.)] ex: CP506
    * Building network router : "B" + [building number] + "RT" ex: B5RT
    * Backbone network router : "MCRT"
    * Access point : "APB" + [building number] + "F" + [building floor] ex: APB5F0
    * Node devices :
      - Laptop : "PCAPB" + [building number] + "F" + [building floor] ex: PCAPB5F0
      - Computer : "PC" + [room number without (.)] + "(VLAN" + [VLAN name] + ")" ex: PC506 (VLAN B5F0)
      - Phone : "VOIP" + [room number without (.)] ex: VOIP506



## Layer 2 Configuration <a name="layer-2-configuration"></a>
* The consolidation points CP506, and CP509 are in the floor 0 section (to the left), and CP512, and CP516 in the floor 1 section (to the right).

* The cable type will also match the ones settled in the previous sprint. Fiber between Intermidiate cross-connects and Copper for everything else.

* Redundant cable link are also used to connect the horizontal cross-connects and the intermidiate cross-connects


### Virtual Lans

Taking from the planning file, the Vlan's name and id are the following:


| Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |   Backbone  |
  | :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------:|
  | 5        | B5F0 - 515  | B4F1 - 516  | B4WF - 517  | B4DMZ - 518 | B4VIP - 519 | B1BBC - 520        

### VLAN VTP configuration

VTP server name used : **rc22nag2**

It is set in every switch of the building where:
  * ICB5 (switch representing Intermidiate cross-connect) is in server mode
  * HCB5F0, HCB5F1, CP506, CP509, CP512, CP516 are in client mode


## Layer 3 Configuration <a name="layer-3-configuration"></a>

### IPv4 Configuration

Taking from the planning file, the IPv4 configurations is the following:


  | Building | Hosts Needed | Hosts Available | Subnet Address  | Mask          |      
  | :------: | :---------:  | :-------------: | :-------------: | :-----------: | 
  | 5        | 200          | 254             | 172.18.189.0/24 | 255.255.255.0	|
  |Backbone  | 120          | 512             | 172.18.190.0/23 | 255.255.254.0	|

### Building 5 IPv4 Configuration
 **172.18.189.0/24**

 The following table has the network IPs for each VLAN needed for Building 4:

  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B4WF  | 60           | 172.18.189.0/26   | 255.255.255.192 | 62              |
  | B4F1  | 55           | 172.18.189.64/26  | 255.255.255.192 | 62              |
  | B4F0  | 40           | 172.18.189.128/26 | 255.255.255.192 | 62              |
  | B4VIP | 25           | 172.18.189.192/27 | 255.255.255.224 | 30              |
  | B4DMZ | 20           | 172.18.189.224/27 | 255.255.255.224 | 30              |


## Router and Static Routing

DHCP pools where used as it allows for dynamic attribution of IPs within the network.