<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 11
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 VMs (Windows & Linux) on a shared VNet
- Install and run Wireshark on the Windows VM
- Simulate traffic (e.g., ping, SSH, DNS, RDP)
- Block/allow traffic via NSG rules
- Observe traffic behavior under different conditions

<h2>Actions and Observations</h2>

**VM SETUP**
![image](https://github.com/user-attachments/assets/6e687120-5174-4c9b-8f3c-858b625de4e1)

- Deploy two VMs in the same resource group, region, and virtual network:
  - Windows 11 (traffic analyzer)
  - Ubuntu 20.04 (Linux server)
- Start both machines


![rdc-window](https://github.com/user-attachments/assets/73af6d64-38a6-466c-bc38-37ba573f0251)

- Open the application Remote Desktop Connection and enter the public IP of our windows VM
- Fill in the username and connect, prompting to fill in this VM's password as well.
- If asked if you want to connect despite certificate errors, hit yes to be signed in

**INSTALL & RUN WIRESHARK**
![wireshark-dwnld](https://github.com/user-attachments/assets/a3eab885-4480-4e50-a4ad-1b923294462d)
- Once signed in go to the browser and search wireshark
- On their site download wireshark, this program monitors and inspects network traffic
- When the download is complete open up the settup and install the application


![start traffic](https://github.com/user-attachments/assets/38349401-03f3-4512-bae7-b85deaa556d2)
- Open wireshark and hit the blue triangle shaped like a shark's fin to begin monitoring traffic
- Shortly after you all network traffic will start being displayed

**ANALYZE ICMP**
![icmp-ping](https://github.com/user-attachments/assets/bb4b2dc5-6799-43cb-bee4-8502e1923dce)
- Now that there is traffic, filter for all ICMP traffic by typing in "icmp" in the search bar at the top of the page
- In order to see the traffic create activity through powershell by pinging the linux VM
- In order to ping the linux VM via powershell type "ping" and the private IP of the linux VM, in this case it is 10.0.0.5
- Wireshark will show the activity from pinging the linux VM, communicating with requests and replies

**BLOCK ICMP WITH NSG**
![image](https://github.com/user-attachments/assets/03820a90-8393-4052-b45e-19048ec2a65a)
- Configure the firewall to deny the ping request and monitor the activity from it
- In Azure go into linux VM's network settings and open the network security group by clicking it
- Block icmp protocol by adding a security rule to deny icmp
- Make sure the priority is before all others to make this is the first rule the firewall follows


![security in effect](https://github.com/user-attachments/assets/ee985b71-95ed-4338-9689-aa5e1b74d34c)
- Now that the firewall has been adjusted to deny icmp, ping request will no longer get replies
- Ping using the private IP again and powershell will display "your request timed out"
- Inside Wireshark you can see the Windows VM sending request to the linux VM but is recieving no replies


![security rule set](https://github.com/user-attachments/assets/6f83e1ac-a591-4276-8cc6-6e29c01a712b)
- To recieve replies delete the rule created to deny ping request

**MONITOR SSH TRAFFIC**
![image](https://github.com/user-attachments/assets/1a59625a-ca12-49e9-b53b-e194ae0fda55)
![image](https://github.com/user-attachments/assets/ee7f5fe8-c2a9-47c8-a746-96a4ece5371c)
- Here is filtering SSH traffic, which can also be done by filtering traffic to port 22(tcp.port==22) since SSH already uses port 22
- In powershell create traffic by logging into the linux server by running "ssh username@private IP", follow with the password create for that VM to be logged in
- Observe the traffic from logging into the linux VM
- Every action will display traffic because they are all being communicated to the linux VM. Every file created, key typed, and everything in between will be displayed.
- Exit your linux connection by typing exit and hitting enter

**MONITOR DHCP REQUEST**
![image](https://github.com/user-attachments/assets/84183f0d-d7aa-43c2-8024-85198f93f024)
- In this image DHCP traffic is being filtered. DHCP uses port 67 and 68 as well so you may filter via those ports as well
- DHCP assigns IP addresses and so generate traffic by requestinga new IP address with "ipconfig /renew"
- In the picture above observe the DHCP discovery, offer, request, and acknowledgment process 

**MONITOR DNS QUERIES**
![image](https://github.com/user-attachments/assets/18c95b1c-4f6b-4e9d-9538-8c1af44b41cb)
- This time were looking at DNS traffic, also visible on port 53
- lookup a domain/website via powershell "nslookup"
- Wireshark displays DNS request and corresponding IP resolution

**MONITOR RDP SESSION**
![image](https://github.com/user-attachments/assets/5ed3efc2-d125-420f-8c18-000057f19ab5)
- Lastly is RDP, using port 3389. filter using tcp.port==3389
- Due to the ongoing RDP usage activity floods the interface with real-time encrypted traffic streams

**This Demonstration focused on network security and administrative task in the cloud, showing:**
- *How NSGs regulate inbound/outbound VM traffic*
- *Protocol behavior under different network configurations*
- *Real-time traffic inspection with Wireshark*
- *Firewall testing, port analysis, and packet structure*
- *This is a great technical piece for demonstrating fundamental network security and system administration knowledge in a cloud-hosted environment.*

