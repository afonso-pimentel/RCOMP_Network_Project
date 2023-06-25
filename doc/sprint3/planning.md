RCOMP 2021-2022 Project - Sprint 3 planning
===========================================
### Sprint master: 1181544 ###

# 1. Sprint's backlog #

  * Implement OSPF to all buildings
  * Implement HTTP and DNS Servers and configure them to all buildings
  * Implement DHCP to all buildings
  * Implement VOIP to all buildings
  * Implement VOIP to all buildings
  * Implement NAT to all buildings
  * Implement ACL firewall to all buildings


# 2. Technical decisions and coordination #

## IPs 
  Since our group already implemented DHCP in the previous sprint we only plan on making small ajustments to it so it works with the requirements of this sprint.
  Since we will also need to consult our ip tabled defined in previous sprint I will copy that part of the planning to this sprint as well.

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

---

* **OSPF**

  For the OSPF we will define each building number as an area so, building 1 will be area 1 and backbone will be 0.

* **VOIP**

  For the voip numbers we need to define a number with the format of 4 digits, so we defined the first digit to the number of building, and the 3 other digits to the numbers of the phone of that building.
  For example first phone of building 4 will have number 4001.


* **NAT**

  For the NAT configuration we decided that each building will have their port 100X1 open for the http server and the X means the number of the building
  For the NAT configuration we decided that each building will have their port 100X1 open for the http server and the X means the number of the building
  For the NAT configuration we decided that each building will have their port 100X3 open for the DNS server and the X means the number of the building

* **ACL**

  Our team decided to seperate our acls in two individual interfaces one for internal access and other for external access.
  Since the access list has to have a number greater than 100 then we decided to alocate the middle digit with the number of the building and the right most digit to the number of interface.
  Our interface 1 is configured to secure the internal connection.
  And our interface 2 if configured to secure the external connection.

---

### Equipment
 
- Router - *2811* 
- Switch - *PT-Empty*
- Desktop - *PC*
- Laptop - *Laptop*
- IP Phone - *7960 model*
- Access Point - *AP-PT*

# Server ips for each building

## Building 1
| Server Type     | IP Address    | MASK Address   |
| :-------------: | :----------:  | :----------:   |
| HTTP            | 172.18.184.131| 255.255.255.128|
| DNS             | 172.18.184.130| 255.255.255.128|

## Building 2
| Server Type     | IP Address    | MASK Address   |
| :-------------: | :----------:  | :----------:   |
| HTTP            | 172.18.186.227| 255.255.255.240|
| DNS             | 172.18.186.226| 255.255.255.240|

## Building 3
| Server Type     | IP Address    | MASK Address   |
| :-------------: | :----------:  | :----------:   |
| HTTP            | 172.18.187.195| 255.255.255.224|
| DNS             | 172.18.187.194| 255.255.255.224|

## Building 4
| Server Type     | IP Address    | MASK Address   |
| :-------------: | :----------:  | :----------:   |
| HTTP            | 172.18.188.243| 255.255.255.240|
| DNS             | 172.18.188.242| 255.255.255.240|

## Building 5
| Server Type     | IP Address    | MASK Address   |
| :-------------: | :----------:  | :----------:   |
| HTTP            | 172.18.189.227| 255.255.255.224|
| DNS             |172.18.189.226 | 255.255.255.224|