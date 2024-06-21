# Simple-Company-Networking-Design
 This case study illustrates the design of a network infrastructure for Initech's new branch. Featured are separate VLANs for Admin/IT, Finance/HR, and Customer Service/Reception departments, Cisco products, automatic IPv4 address assignment, and inter-department communication.

# Networking Case Study:  Unifying Departments at a Company's Branch


## Introduction
Initech, a rapidly expanding enterprise headquartered in Eastern Australia, has a global customer base exceeding 2 million. Specializing in the buying and selling of food items, the company is now poised to establish a branch near the local village of Bonalbo. In this endeavor, the company seeks the expertise of IT professionals to architect and implement a network infrastructure for the new branch. This network is envisioned to function autonomously from the HQ network, catering to the specific operational needs of the branch's three departments: Admin/IT, Finance/HR, and Customer Service/Reception.

**Project Objectives:**
1. Design and implement a network infrastructure for the Bonalbo branch.
2. Create separate VLANs for each department.
3. Establish wireless networks for user connectivity in each department.
4. Ensure automatic assignment of IPv4 addresses to host devices.
5. Facilitate seamless communication among devices in all departments.

## Network Topology
![Network Topology](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/e45cfbae-d846-4a71-b7cf-90bb230f9917)


## Subnetting

### Subnet Calculation
- Number of subnets: 3
- No. of subnets = 2^n
- 2^n = 3 ⇒ n = 2

### Network Address Subnetting
- Base Network Address: 192.168.1.0
- Created 3 subnets: Admin/IT, Finance/HR, and CS/Reception
- Subnet Mask Calculation: 
  - 2^n = number of subnets (3), n = 2
      - The value of n is what will represent the number of borrowed network bits in a Class C address, 255.255.255.0. This gives us the following in binary:
        - Binary: 11111111.11111111.11111111.1100000
  - When converted from binary: Subnet Mask = 255.255.255.192
    - Block Size: 64

### Subnet Details

#### Admin/IT
- Subnet Mask: 255.255.255.192
- Network ID: 192.168.1.0
- Valid Host Range: 192.168.1.1 - 192.168.1.62
- Broadcast ID: 192.168.1.63

We calculate the subnet for ADMIN/IT by looking at our binary subnet mask, 11111111.11111111.11111111.10000000 (255.255.255.192). We have 3 octets of 8, and one octet of 2, giving us a total of 26. 
  - The Network address and subnet mask is 192.168.1.0/26. The aforementioned binary subnet mask represents this notation.


#### Finance/HR
- Subnet Mask: 255.255.255.192
- Network ID: 192.168.1.64
- Valid Host Range: 192.168.1.65 - 192.168.1.126
- Broadcast ID: 192.168.1.127

  - Our subnet mask for FINANCE/HR is the same with the difference in our Network ID. 
    - The Network address and subnet mask is 192.168.1.64/26.


#### CS/Reception
- Subnet Mask: 255.255.255.192
- Network ID: 192.168.1.128
- Valid Host Range: 192.168.1.129 - 192.168.1.190
- Broadcast ID: 192.168.1.191

  - Our subnet mask for CS/RECEPTION is the same with the difference in our Network ID. 
    - The Network address and subnet mask is 192.168.1.128/26.


## VLAN Configuration

### Admin/IT
- Interfaces: PC0, Printer0, and AccessPoint0 (fa0/2-4)
- VLAN 10 created with switchport access mode

### Finance/HR
- Interfaces: PC1, Printer1, and AccessPoint1 (fa0/5-7)
- VLAN 20 created with switchport access mode

### CS/Reception
- Interfaces: PC2, Printer2, and AccessPoint2 (fa0/8-10)
- VLAN 30 created with switchport access mode

![config switch](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/76fb6ae2-5e43-4f61-8f26-4e0f0395fca7)
![do sh start](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/5af34d91-24e6-41b0-8e2e-95d4e5945e79)


## Wireless Network Configuration

### Admin/IT
- SSID: Admin-WiFi
- WPA2-PSK: Admin@123

![config admin AP](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/53ba7d6c-eeea-4f10-9492-c929c49338e3)

### Finance/HR
- SSID: Finance-WiFi
- WPA2-PSK: Finance@123

![config finance AP](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/69b3ca6d-afa1-45c6-8559-6f7a9a92b140)

### CS/Reception
- SSID: CS-WiFi
- WPA2-PSK: Customer@123

![config cs AP](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/36fde70d-5948-45d5-bcbd-0c4f566cc35e)

## Switch Configuration
- Enable trunk interface for transmission of traffic from multiple VLANs over a single physical link:
    - interface fa0/1
    - switchport mode trunk

![trunk config](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/afd81de6-0db5-45d9-922f-bd029f5df5ce)

 
## Router Configuration

### Interface Configuration
- For router configuration, we start be turning on the interface as it’s shutdown by default. We use the following commands:
    - interface gig0/0
    - no shutdown
    - write memory

![enable router](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/f6954f11-a338-4be9-8a54-297effe1535d)


 ### Inter-VLAN Routing
- Create subinterfaces and assign IP addresses to act as the default gateway for respective VLANs.

#### VLAN 10 (Admin/IT)

![enable vlan 10](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/a585d85e-e1c9-4360-9ee4-6a9d90000c16)

#### VLAN 20 (Finance/HR)

![enable config vlan 20](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/f77480f0-a76d-44af-abdb-e664bb9508ed)

#### VLAN 30 (CS/Reception)

![enable-config vlan 30](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/98bd804f-8163-423c-aeb5-e0423cf7e4c3)

![3 sub int from phys int](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/3cc88b81-e98e-4842-9010-97a84ec66412)


### DHCP Server Configuration
    - Enable DHCP service on the device (enabled by default on cisco devices) and create pools
    - Pools are created for the three subnets: Admin, Finance and CS/Reception
        - Network address are assigned to each pool


#### Admin

![create admin pool](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/816ac25c-ec77-48b8-929e-f813e2a9f2b8)

![admin pc0 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/20a83507-1200-426f-b222-82d1fa303228)

![admin printer0 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/5834ef97-9d46-4f3f-9e2a-3885ad19dac5)


#### Finance

![create finance pool](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/a3806f1f-815a-4315-922a-4a7e2b4c3120)

![finance pc1 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/cb1822fc-0a54-477b-8005-48139ba202ae)

![finance printer1 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/1a4275ee-e3c5-4a86-82c9-8253a9b3c4b8)


#### CS/Reception

![create cs pool](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/7c6565af-175e-48c3-a802-4200cb204891)

![cs pc2 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/8e8969c1-100c-439f-898e-d07925fc8078)

![cs printer2 dhcp successful](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/4394c5c8-2917-43d8-bbf3-5107f9770054)



## Testing Connectivity
- All requirements are now satisfied through our configuration. Before testing connectivity  between devices:
    - Ensure all devices have the IP allocation method set to DHCP.
    - Place a smartphone and laptop in each subnet and connect to the respective access points.

#### Smartphone0
To connect to AccessPoint0, we enter the SSID for Admin-WiFi and pass phrase for the WPA2-PSK: Admin@123
Let’s also review the IP configuration and ensure we have DHCP set.

![smartphone ap access](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/3e9c3f2a-2eff-4964-9318-f603a089d2eb)

![smartphone assigned ip ](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/be8ac5ca-1ff2-45b3-9a68-d5301dbc2ee1)


#### Laptop1
To connect the laptop, first remove the existing module and replace it with a wireless module.
We connect to the Finance-Wifi through PC Wireless
The connection is successful

![laptop1 connected to AP](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/ab7a65a2-eb91-4d2d-90ee-472965c03297)

![laptop1 IP config](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/f41642fe-4d45-431e-9d19-07b4d08617e8)


#### Tablet
To connect to AccessPoint2, we enter the SSID for CS-WiFi and pass phrase for the WPA2-PSK: Customer@123
Let’s also review the IP configuration and ensure we have DHCP set.

![tablet connection to AP](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/8a7b455c-6ff3-4816-88a2-f70ac63bcac4)



### Example Tests

#### Smartphone0 to PC2
    - ping 192.168.1.131
        - successful!

![smph ping pc2](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/1829f13a-e03c-456c-a574-dbff5d38cfd1)

#### Tablet to Printer0
    - ping 192.168.1.2
        - successful!

![tablet ping printer0](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/dc5af091-430e-499e-9b8e-1d0fc40713be)

#### Laptop1 to PC0
    - ping 192.168.1.3
        - successful!

![laptop1 ping pc0](https://github.com/irioshi/Simple-Company-Networking-Design/assets/98143751/6bfcb750-e319-4725-9bd8-403a9d5d64e3)



