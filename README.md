# Financial-Institution-Network-Simulation-GNS3-
## About the project

This project is a simulation of a small financial institution network built in GNS3. The goal was to practice designing and configuring an enterprise network instead of configuring individual devices separately.
The network consists of a headquarters and one branch office connected through a WAN link. Different departments are separated into VLANs and communicate through a router-on-a-stick configuration.
Besides routing and switching, I also configured several common enterprise services such as DNS, DHCP, NTP and Syslog using an Ubuntu server.

## Network topology

![network topolgy](https://github.com/akyllaii/Financial-Institution-Network-Simulation-GNS3-/blob/main/images/topology.png)

## Features
* VLAN segmentation
* Inter-VLAN routing
* OSPF between HQ and Branch
* DHCP
* Internal DNS server (Bind9)
* SSH management
* NTP server
* Syslog server
* ACLs between departments
* Port Security
* DHCP Snooping
* Dynamic ARP Inspection (DAI)

## IP Addressing 

|    VLAN     |     Network   | Purpose  | 
| ----------- | ------------- | --------------- | 
| 10   | 10.10.10.0/24    | HR   |
| 20      | 10.10.20.0/24    | Finance     | 
| 30  | 10.10.30.0/24    | IT   | 
| 40  | 10.10.40.0/24    | Guest | 
| 99  | 10.10.99.0/24    | Management   | 
| 50  | 10.20.50.0/24    | Branch   | 
| WAN Link 1  | 172.16.0.0/30    | HQ Router $\leftrightarrow$ ISP Link   | 
| WAN Link 2  | 172.16.0.4/30    | Branch Router $\leftrightarrow$ ISP Link   | 

## Technologies

* Cisco IOS
* GNS3
* Ubuntu Desktop
* Bind9
* Chrony
* rsyslog
* Wireshark

## Getting Started

### Prerequisites
Before importing the project, make sure you have:
* GNS3 installed
* Cisco IOS images for the routers and switches used in this topology
* An Ubuntu Desktop or Ubuntu Server VM
* GNS3 VM (recommended)
### Clone the repository
```
git clone https://github.com/akyllaii/Financial-Institution-Network-Simulation-GNS3-.git
cd financial-network-gns3
```
### Import the topology
1. Open GNS3.
2. Select File → Open Project.
3. Open the .gns3 project file from the topology/ directory.
4. If prompted, assign the required Cisco IOS images and the Ubuntu VM.
### Start the project
1. Start all routers, switches, and the Ubuntu VM.
2. Wait until OSPF neighbors are established.
3. Verify that the Ubuntu services (DNS, NTP, and Syslog) are running.
### Basic verification
Run a few quick tests to confirm everything is working:
* Ping between HQ and Branch devices.
* Verify DNS resolution:
```
nslookup server.bank.local 10.10.99.10
```
* Check OSPF neighbors:
```
show ip ospf neighbor
```
* Check NTP status:
```
show ntp status
```
* Connect to a device using SSH.

If any services are not working, compare the device configurations in the configs/ directory with your running configuration.

## Author's Note

This project took much longer than I originally expected because most of the work wasn't configuring devices—it was troubleshooting how different technologies interacted with each other.

One of the biggest challenges was debugging ACLs that were unintentionally blocking legitimate traffic. Since multiple ACLs were applied to different interfaces, it wasn't always obvious where packets were being dropped, so I had to verify routing, test connectivity between VLANs, and review ACL matches step by step.

Another challenge was NAT. At one point, internal traffic between the HQ and Branch networks was being translated unexpectedly, which affected communication with services such as NTP. Finding the cause required using ` show ` commands on the Cisco devices together with ` tcpdump ` on the Ubuntu server to trace packets and understand where they were being modified.

Although troubleshooting these issues was sometimes frustrating, it helped me better understand how routing, NAT, ACLs, and network services work together in an enterprise environment.

This project is still evolving, and I plan to expand it by adding features such as HSRP, a DMZ, and network monitoring
