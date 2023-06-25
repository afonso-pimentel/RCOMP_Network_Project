RCOMP 2021-2022 Project - Sprint 2 planning
===========================================
### Sprint master: 1181350 ###

# 1. Sprint's backlog #

  * Development of a layer two and layer three Packet Tracer simulation for building one, encompassing the campus backbone. Integration of every team member Packet Tracer simulation into a single simulation.
  * Development of a layer two and layer three Packet Tracer simulation for building two, encompassing the campus backbone.
  * Development of a layer two and layer three Packet Tracer simulation for building three, encompassing the campus backbone.
  * Development of a layer two and layer three Packet Tracer simulation for building four, encompassing the campus backbone.
  * Development of a layer two and layer three Packet Tracer simulation for building five, encompassing the campus backbone.

# 2. Technical decisions and coordination #

## IPs 

Since the reserved ip for this group is 172.18.186.0/21
We can easily know how many available hosts we have, by representing the subnet mask:
255.255.11111000.00000000

The /21 is all of the bits that represent the network we are in, given that
the subnet mask only has 32 bits. Since this is the case, 32-21 = 11, so we have 11 bits
to represent the hosts. 11 bits allow us to represent up to 2047 hosts, or 2046 since there is always one reserved as the broadcast ip.

We have 2046 available hosts for our campus. We need to divide it wisely by building,
not all buildings have the same need.

Let's analyze the needed values:

  | Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |  Total  |
  | :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :-----: |
  | 1        | 60          | 80          | 120         | 100         | 40          | **400** |
  | 2        | 25          | 50          | 120         | 12          | 12          | **219** |
  | 3        | 35          | 45          | 55          | 28          | 25          | **188** |
  | 4        | 28          | 55          | 70          | 10          | 12          | **175** |
  | 5        | 40          | 55          | 60          | 20          | 25          | **200** |
  |Backbone  |             |             |             |             |             | **120** |

It all adds up to a total of 1302.
We do need the /21 because if we only had /22, we would only have 1022 available hosts, way below
the needed values.

How shall we divide it?
Let's analyze the following table:

  | Building | Hosts Needed | Hosts Available | Subnet Address  | Mask          |      
  | :------: | :---------:  | :-------------: | :-------------: | :-----------: | 
  | 1        | 400          | 510             | 172.18.184.0/23 | 255.255.254.0	|
  | 2        | 219          | 254             | 172.18.186.0/24 | 255.255.255.0	|
  | 3        | 188          | 254             | 172.18.187.0/24 | 255.255.255.0	|
  | 4        | 175          | 254             | 172.18.188.0/24 | 255.255.255.0	|
  | 5        | 200          | 254             | 172.18.189.0/24 | 255.255.255.0	|
  |Backbone  | 120          | 512             | 172.18.190.0/23 | 255.255.254.0	|

We must always have the subnet address that allows us to have at least the needed hosts + 2 (the reserved hosts).
So, for example, in building 1, if we require 400 we will need the subnet address right above that need:

172.18.184.0/23 with the mask 255.255.254.0 which translates into 255.255.11111110.00000000.

Binary representation makes it easier for us to evaluate how many available hosts we will have. 
What will we get with 9 bits to 1? We get the number 511, meaning we have 510 available hosts. Does it "feed" the need of 400 hosts? Yes.

We conclude that for building 1, we will take up 1/4 of the available hosts.
We still have 3/4 to divide between the other 4 buildings and the backbone.
So, now we will start again from the address 172.18.186.0. We can still use up to the address 172.18.191.254.

For building 2, we will only need 219 hosts, so the subnet mask must be 255.255.255.0, to give us 254 available hosts, represented by 172.18.186.0/24.

We continue like this up to the backbone which will need 120 hosts but, it will get the remaining part of the address range,there's no need to divide further, hence having the address 172.18.190.0/23 with the mask 255.255.254.0.

**Important note:** In this sprint, asside from the backbone network, all other network IPs will be assigned dynamicaly using DHCP, this was not a requirement but the team consulted with the Teacher and agreed on it.

## VLAN Names and IDs

### **VTP**

All VLANs are in a VTP server with the domain name *rc22nag2*

### **Tags Available**

All VLANs names should start with the prefix B and the building number, for example:
*B5* -> Building 5 

And are followed by the following suffix:

- *F0* -> Ground Floor VLAN
- *F1* -> Floor One VLAN
- *WF* -> Wi-fi VLAN 
- *DMZ* -> DMZ VLAN
- *VIP* -> VoIP VLAN
- *BBC* -> Campus Backbone VLAN

### **IDs Available**

Our group can use the VLAN ids from 495 to 525. 

Each building will have its own reserved five ids. 

### **Names and IDs Distribution** 

  | Building | Ground Floor|    Floor 1  | Wi-Fi       | DMZ         | Voip        |   Backbone  |
  | :------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: |
  | 1        | B1F0 - 495  | B1F1 - 496  | B1WF - 497  | B1DMZ - 498 | B1VIP - 499 | B1BBC - 520 |
  | 2        | B2F0 - 500  | B2F1 - 501  | B2WF - 502  | B2DMZ - 503 | B2VIP - 504 |             |
  | 3        | B3F0 - 505  | B3F1 - 506  | B3WF - 507  | B3DMZ - 508 | B3VIP - 509 |             |
  | 4        | B4F0 - 510  | B4F1 - 511  | B4WF - 512  | B4DMZ - 513 | B4VIP - 514 |             |
  | 5        | B5F0 - 515  | B5F1 - 516  | B5WF - 517  | B5DMZ - 518 | B5VIP - 519 |             |

## VLAN IPs

In each VLAN the first ip available is reserved for the router. So all the buildings have the same structure to facilitate the integration.

### Backbone - *172.18.190.0/23*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------- | :-------------: |
  | B1BBC | 120          | 172.18.190.0/23   | 255.255.254.0   | 510             |

### Building 1 - *172.18.184.0/23*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------- | :-------------: |
  | B1WF  | 120          | 172.18.184.0/25   | 255.255.255.128 | 126             |
  | B1DMZ | 100          | 172.18.184.128/25 | 255.255.255.128 | 126             |
  | B1F1  | 80           | 172.18.185.0/25   | 255.255.255.128 | 126             |
  | B1F0  | 60           | 172.18.185.128/26 | 255.255.255.192 | 62              |
  | B1VIP | 40           | 172.18.185.192/26 | 255.255.255.192 | 62              |

### Building 2 - *172.18.186.0/24*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B2WF  | 120          | 172.18.186.0/25   | 255.255.255.128 | 126             |
  | B2F1  | 50           | 172.18.186.128/26 | 255.255.255.192 | 62              |
  | B2F0  | 25           | 172.18.186.192/27 | 255.255.255.224 | 30              |
  | B2DMZ | 12           | 172.18.186.224/28 | 255.255.255.240 | 14              |
  | B2VIP | 12           | 172.18.186.240/28 | 255.255.255.240 | 14              |

### Building 3 - *172.18.187.0/24*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B3WF  | 55           | 172.18.187.0/26   | 255.255.255.192 | 62              |
  | B3F1  | 45           | 172.18.187.64/26  | 255.255.255.192 | 62              |
  | B3F0  | 35           | 172.18.187.128/26 | 255.255.255.192 | 62              |
  | B3DMZ | 28           | 172.18.187.192/27 | 255.255.255.224 | 30              |
  | B3VIP | 25           | 172.18.187.224/27 | 255.255.255.224 | 30              |

### Building 4 - *172.18.188.0/24*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B4WF  | 70           | 172.18.188.0/25   | 255.255.255.128 | 126             |
  | B4F1  | 55           | 172.18.188.128/26 | 255.255.255.192 | 62              |
  | B4F0  | 28           | 172.18.188.192/27 | 255.255.255.224 | 30              |
  | B4VIP | 12           | 172.18.188.224/28 | 255.255.255.240 | 14              |
  | B4DMZ | 10           | 172.18.188.240/28 | 255.255.255.240 | 14              |

### Building 5 - *172.18.189.0/24*
  | VLAN  | Hosts Needed | Subnet Address    |  Mask           | Hosts Available |
  | :---- | :----------: | :---------------- | :-------------: | :-------------: |
  | B5WF  | 60           | 172.18.189.0/26   | 255.255.255.192 | 62              |
  | B5F1  | 55           | 172.18.189.64/26  | 255.255.255.192 | 62              |
  | B5F0  | 40           | 172.18.189.128/26 | 255.255.255.192 | 62              |
  | B5VIP | 25           | 172.18.189.192/27 | 255.255.255.224 | 30              |
  | B5DMZ | 20           | 172.18.189.224/27 | 255.255.255.224 | 30              |

# Routing Tables

To simplify, we will send the package to the Campus router, and then it will know where to send it, according to this routing table:

| Network Address | Via          |
| :-------------: | :----------: |
| 172.18.184.0/23 | 172.18.190.6 |
| 172.18.186.0/23 | 172.18.190.2 |
| 172.18.187.0/23 | 172.18.190.3 |
| 172.18.188.0/23 | 172.18.190.4 |
| 172.18.189.0/23 | 172.18.190.5 |
| 0.0.0.0/0       | 15.203.48.170|

Perhaps the defined IPs per router could have been different, but the following was agreed upon:
| Router ID       | Address      |
| :-------------: | :----------: |
| Router IC Building 1| 172.18.190.6 |
| Router IC Building 2 | 172.18.190.2 |
| Router IC Building 3 | 172.18.190.3 |
| Router IC Building 4 | 172.18.190.4 |
| Router IC Building 5 | 172.18.190.5 |
| ISP Router      | 15.203.48.170|

And then every one of these routers from building 1 to 5 will have the following static route:

| Network Address | Via          |
| :-------------: | :----------: |
| 0.0.0.0/0 | 172.18.190.1 |

This means that for any address it will send it to the Campus router, and it will then run through the initial routing table.

## Equipment & Naming

### Naming

In each building the suggested naming for switches follows the naming tag:
- B{*building number*}F{*floor number*}_{*IC|HC|CP|MC*}

*Examples:* **B1F0_IC | B1F1_HC | B1F1_CP1**

### Equipment
 
- Router - *2811* 
- Switch - *PT-Empty*
- Desktop - *PC*
- Laptop - *Laptop*
- IP Phone - *7960 model*
- Access Point - *AP-PT*

# 3. Subtasks assignment #

  * 1151399 - Development of a layer two and layer three Packet Tracer simulation for building one, encompassing the campus backbone. Integration of every team member Packet Tracer simulation into a single simulation.
  * 1181350 - Development of a layer two and layer three Packet Tracer simulation for building two, encompassing the campus backbone.
  * 1181357 - Development of a layer two and layer three Packet Tracer simulation for building three, encompassing the campus backbone.
  * 1181544 - Development of a layer two and layer three Packet Tracer simulation for building four, encompassing the campus backbone.
  * 1181654 - Development of a layer two and layer three Packet Tracer simulation for building five, encompassing the campus backbone.