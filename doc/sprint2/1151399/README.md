# RCOMP 2021-2022 Project - Sprint 2 - Building 1 - 1151399
===========================================


## Analysing Steps for Building 1

* [General Information](#general-information) 
* [Layer 2 Configuration](#layer-2-configuration) 
* [Layer 3 Configuration](#layer-3-configuration) 


---
## General Information <a name="general-information"></a>
* Packet Tracer Version 
    8.1.1.0022
* Device naming
    My devices follow this naming convention:
    * Type of device - IC/Router/HC/CP/PC/IP
    * Building Name/Number(if apllied) - B1
    * Floor the device is on(if applied) - F0/F1
    * Room the device is in (if it is not an unique device) - 1.0.2, 1.0.3, and so on.
    

## Layer 2 Configuration <a name="layer-2-configuration"></a>
* Every cross-connect and consolidation point is represented in the Packet tracer file with their correct name and position. So for building 1 we can expect the CP from room 1.0.3 to be represented as CP1.0.3, for example. I have used the floors as a background image for better guidance.

* The cable type will also match the ones settled in the previous sprint. Fiber from the routers to the MC and IC switches, from the MC switch to the IC switch and Copper for everything else.

* Redundant cable links are also used as it is shown in the image below

![redundency_ptk](Imgs/redundency_ptk.png)

### Virtual Lans

To make the work easier for us we decided to group all the VLANS and ips configuration in our planning markdown file.

These are the designated VLANS and its IDs for Building 1

| Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |   Backbone  |
  | :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: |
  | 1        | B1F0 - 495  | B1F1 - 496  | B1WF - 497  | B1DMZ - 498 | B1VIP - 499 | B1BBC - 520 | 

### VLAN VTP configuration

Our team decided to call our domain **rc22nag2** so we all have the same domain throughtout our entire infractructure including our backbone.

We also made sure that our Server Switches had all the infractructure VLANS saved as it was stated as a requirment.

Building 1 has its Intermediate Cross-connect switch as a Server Switch and the other switches (Horizontal Cross-connect and Consolidation points) are clients of that server switch.


## Layer 3 Configuration <a name="layer-3-configuration"></a>

### IPv4 Configuration

In order to facilitate our work we established our IPv4 configurations together as a team and therefore we have the whole building ip configuration in our planning markdown file as well.

The following table has the number of Hosts needed for Building 4 and backbone:

  | Building | Hosts Needed | Hosts Available | Subnet Address  | Mask          |      
  | :------: | :---------:  | :-------------: | :-------------: | :-----------: | 
  | 1        | 400          | 502             | 172.18.184.0/23 | 255.255.254.0	|
  |Backbone  | 120          | 512             | 172.18.190.0/23 | 255.255.254.0	|

### Building 1 IPv4 Configuration
 **172.18.184.0/23**

 The following table has the network IPs for each VLAN needed for Building 1:

  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------- | :-------------: |
  | B1WF  | 120          | 172.18.184.0/25   | 255.255.255.128 | 126             |
  | B1DMZ | 100          | 172.18.184.128/25 | 255.255.255.128 | 126             |
  | B1F1  | 80           | 172.18.185.0/25   | 255.255.255.128 | 126             |
  | B1F0  | 60           | 172.18.185.128/26 | 255.255.255.192 | 62              |
  | B1VIP | 40           | 172.18.185.192/26 | 255.255.255.192 | 62              |

  In order to organize and have a good notion on which port and which equipment would be operating in each VLAN, I created this table:
| VLAN Name        | 495 - B1F0 | Port     | 496 - B1F1 | Port    | 497 - B1WF | Port       | 498 - B1DMZ | Port     | 499 - B1VIP | Port         | 520 - B1BBC | Port |
| ---------------- | ---------- | -------- | ---------- | ------- | ---------- | ---------- | ----------- | -------- | ----------- | ------------ | ----------- | ---- |
| Devices And Port | MC         |          | MC         |         | MC         |            | MC          |          | MC          |              | MC          |      |
|| IC               |            | IC       |            | IC      |            | IC         |             | IC       |             | IC           |             |
|| HCB1F0           |            | HCB1F0   |            | HCB1F0  |            | HCB1F0     |             | HCB1F0   |             | HCB1F0       |             |
|| HCB1F1           |            | HCB1F1   |            | HCB1F1  |            | HCB1F1     |             | HCB1F1   |             | HCB1F1       |             |
|| All CPs          |            | All CPs  |            | All CPs |            | All CPs    |             | All CPs  |             | All CPs      |             |
|| CP1.0.3          | 1/1        | CP1.1.2  | 1/1        | CP1.0.8 | 2/1        | PCICB1     | 3/1         | CP1.0.3  | 2/1         | RouterCampus | 0/3/0.1     |
|| CP1.0.4          | 1/1        | CP1.1.3  | 1/1        | CP1.1.4 | 2/1        | ServerICB1 | 2/1         | CP1.0.4  | 2/1         |              |             |
|| CP1.0.8          | 1/1        | CP1.1.4  | 1/1        |         |            | PCHCB1F0   | 7/1         | CP1.0.8  | 3/1         |              |             |
|| CP1.0.9          | 1/1        | CP1.1.14 | 1/1        |         |            | PCHCB1F1   | 5/1         | CP1.0.9  | 2/1         |              |             |
|| CP1.0.10         | 1/1        |          |            |         |            |            |             | CP1.0.10 | 2/1         |              |             |
|                  |            |          |            |         ||            |            |             | CP1.1.2  | 2/1         |              |             |
|                  |            |          |            |         ||            |            |             | CP1.1.3  | 2/1         |              |             |
|                  |            |          |            |         ||            |            |             | CP1.1.4  | 3/1         |              |             |
|                  |            |          |            |         ||            |            |             | CP1.1.14 | 2/1         |              |             |

Obviously every switch contains every VLAN in its database and in the ports where they get connected to eachother, they will be trunked.

## Router and Static Routing

Even though it wasn't a requirement, our team decided to configure DHCP pools for each of our VLANS so we could avoid mistakes assigning IPs to our machines and also known Packet Tracers bugs.

In that sense, a distribution made by DHCP protocol was the following:
| PC         | DHCP Assigned Address |
| ---------- | --------------------- |
| PC1.0.3    | 172.18.185.131        |
| PC1.0.4    | 172.18.185.132        |
| PC1.0.8    | 172.18.185.133        |
| PC1.0.9    | 172.18.185.134        |
| PC1.0.10   | 172.18.185.130        |
| PC1.1.2    | 172.18.185.2          |
| PC1.1.3    | 172.18.185.3          |
| PC1.1.4    | 172.18.185.4          |
| PC1.1.14   | 172.18.185.5          |
| PCAPB1F0   | 172.18.184.2          |
| PCAPB1F1   | 172.18.184.3          |
| IPCP1.0.3  | 172.18.185.194        |
| IPCP1.0.4  | 172.18.185.195        |
| IPCP1.0.8  | 172.18.185.196        |
| IPCP1.0.9  | 172.18.185.198        |
| IPCP1.0.10 | 172.18.185.197        |
| IPCP1.1.2  | 172.18.185.199        |
| IPCP1.1.3  | ?                     |
| IPCP1.1.4  | 172.18.185.202        |
| IPCP1.1.14 | 172.18.185.200        |

For IPCP1.1.3 I am unable to understand why not IP was given even though the configuration of the VLAN for the port where that phone is connected is correct.
Every other equipment, as you can see, has an IP that goes according to the ranges defined above in [Layer 3 Configuration](#layer-3-configuration).

_____

# Connections between equipments
Being the one responsible for Building 1 and ensembling the whole Campus' network, I must say that maybe not all of my configurations are correct.

I must start by establishing that we started working on this sprint right from the first day, creating the initial structure we thought was needed for each building. 

Only after a week or so did we realise that maybe we should try and reduce the number of equipments due to the fact that Packet Tracer might crash with so many elements. Nevertheless, I maintained what I had since I had already lost quite some time configuring everything and joining an excel file with every data and command needed.

Taking this into consideration, we can check the tables below to understand the basis behind this configuration.


## Floor 0

| From            | Connector Type  | Port       | To              | Connector Type  | Port |
| --------------- | --------------- | ---------- | --------------- | --------------- | ---- |
| RouterCampus    | GigabitEthernet | 0/1/0      | MC              | GigabitEthernet | 3/1  |
|| GigabitEthernet | 0/3/0           | MC         | GigabitEthernet | 2/1             |
| RouterB1        | GigabitEthernet | 0/1/0      | ICB1            | GigabitEthernet | 6/1  |
|| GigabitEthernet | 0/3/0           | ICB1       | GigabitEthernet | 7/1             |
| MC              | GigabitEthernet | 9/1        | ICB1            | GigabitEthernet | 9/1  |
| ICB1            | FastEthernet    | 0/1        | HCB1F0          | FastEthernet    | 0/1  |
|| FastEthernet    | 2/1             | ServerICB1 | FastEthernet    | 0               |
|| FastEthernet    | 3/1             | PCICB1     | FastEthernet    | 0               |
| HCB1F0          | FastEthernet    | 1/1        | CP1.0.3         | FastEthernet    | 0/1  |
|| FastEthernet    | 2/1             | CP1.0.4    | FastEthernet    | 0/1             |
|| FastEthernet    | 3/1             | CP1.0.8    | FastEthernet    | 0/1             |
|| FastEthernet    | 4/1             | CP1.0.9    | FastEthernet    | 0/1             |
|| FastEthernet    | 5/1             | CP1.0.10   | FastEthernet    | 0/1             |
|| FastEthernet    | 6/1             | HCB1F1     | FastEthernet    | 6/1             |
|| FastEthernet    | 7/1             | PCHCB1F0   | FastEthernet    | 0               |
| CP1.0.3         | FastEthernet    | 1/1        | PC1.0.3         | FastEthernet    | 0    |
|| FastEthernet    | 2/1             | IPCP1.0.3  | Switch          |                 |
| CP1.0.4         | FastEthernet    | 1/1        | PC1.0.4         | FastEthernet    | 0    |
|| FastEthernet    | 2/1             | IPCP1.0.4  | Switch          |                 |
| CP1.0.8         | FastEthernet    | 1/1        | PC1.0.8         | FastEthernet    | 0    |
|| FastEthernet    | 2/1             | APB1F0     | Port            | 0               |
|| FastEthernet    | 3/1             | IPCP1.0.8  | Switch          |                 |
| CP1.0.9         | FastEthernet    | 1/1        | PC1.0.9         | FastEthernet    | 0    |
|| FastEthernet    | 2/1             | IPCP1.0.9  | Switch          |                 |
| CP1.0.10        | FastEthernet    | 1/1        | PC1.0.10        | FastEthernet    | 0    |
|| FastEthernet    | 2/1             | IPCP1.0.10 | Switch          |                 |
| PCAPB1F0        | Wireless        |            | APB1F0          | Wireless        |      |

## Floor 1

| From         | Connector Type | Port       | To           | Connector Type | Port |
| ------------ | -------------- | ---------- | ------------ | -------------- | ---- |
| ICB1         | FastEthernet   | 1/1        | HCB1F1       | FastEthernet   | 0/1  |
| HCB1F1       | FastEthernet   | 1/1        | CP1.1.2      | FastEthernet   | 0/1  |
|| FastEthernet | 2/1            | CP1.1.3    | FastEthernet | 0/1            |
|| FastEthernet | 3/1            | CP1.1.4    | FastEthernet | 0/1            |
|| FastEthernet | 4/1            | CP1.1.14   | FastEthernet | 0/1            |
|| FastEthernet | 5/1            | PCHCB1F1   | FastEthernet | 0              |
|| FastEthernet | 6/1            | HCB1F0     | FastEthernet | 6/1            |
| CP1.1.2      | FastEthernet   | 1/1        | PC1.1.2      | FastEthernet   | 0    |
|| FastEthernet | 2/1            | IPCP1.1.2  | Switch       | 0              |
| CP1.1.3      | FastEthernet   | 1/1        | PC1.1.3      | FastEthernet   | 0    |
|| FastEthernet | 2/1            | IPCP1.1.3  | Switch       | 0              |
| CP1.1.4      | FastEthernet   | 1/1        | PC1.1.4      | FastEthernet   | 0    |
|| FastEthernet | 2/1            | APB1F1     | Port         | 0              |
|| FastEthernet | 3/1            | IPCP1.1.4  | Switch       | 0              |
| CP1.1.14     | FastEthernet   | 1/1        | PC1.1.14     | FastEthernet   | 0    |
|| FastEthernet | 2/1            | IPCP1.1.14 | Switch       | 0              |
| PCAPB1F1     | Wireless       |            | APB1F01      | Wireless       |      |

## Redundencies between buildings

| From | Connector Type  | Port | To   | Connector Type  | Port |
| ---- | --------------- | ---- | ---- | --------------- | ---- |
| ICB1 | GigabitEthernet | 8/1  | ICB5 | GigabitEthernet | 5/1  |
| ICB2 | GigabitEthernet | 5/1  | ICB1 | GigabitEthernet | 5/1  |
| ICB3 | GigabitEthernet | 5/1  | ICB2 | GigabitEthernet | 6/1  |
| ICB4 | GigabitEthernet | 6/1  | ICB3 | GigabitEthernet | 6/1  |
| ICB5 | GigabitEthernet | 6/1  | ICB4 | GigabitEthernet | 5/1  |

## Connections of other buildings' IC to their respective routers

| From            | Connector Type  | Port            | To       | Connector Type  | Port  |
| --------------- | --------------- | --------------- | -------- | --------------- | ----- |
| ICB1            | GigabitEthernet | 6/1             | RouterB1 | GigabitEthernet | 0/1/0 |
|| GigabitEthernet | 7/1             || GigabitEthernet | 0/3/0    |
| ICB2            | GigabitEthernet | 8/1             | RouterB2 | GigabitEthernet | 0/1/0 |
|| GigabitEthernet | 7/1             || GigabitEthernet | 0/3/0    |
| ICB3            | GigabitEthernet | 8/1             | RouterB3 | GigabitEthernet | 0/1/0 |
|| GigabitEthernet | 7/1             || GigabitEthernet | 0/3/0    |
| ICB4            | GigabitEthernet | 8/1             | RouterB4 | GigabitEthernet | 0/1/0 |
|| GigabitEthernet | 7/1             || GigabitEthernet | 0/3/0    |
| ICB5            | GigabitEthernet | 8/1             | RouterB5 | GigabitEthernet | 0/1/0 |
|| GigabitEthernet | 7/1             || GigabitEthernet | 0/3/0    |

For the tables with the commands used, and in order not to condense this README too much, check out [Commands](COMMANDS.md).