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

- On Azure create 2 VM's within a shared network and resource group to simulate internal network communication across Windows and Linux systems
- Use wireshark to capture and analyze traffic between public domains and the other VM to observe network behavior via packets
- Configure firewall to block and re-enable traffic, displaying how Network Security Groups control VM communication
- Observe traffic for SSH, DHCP, DNS, and RDP to analyze how different protocols behave and interact in a network session
<h2>Actions and Observations</h2>

![image](https://github.com/user-attachments/assets/6e687120-5174-4c9b-8f3c-858b625de4e1)

- Create 2 VM's, 1 using windows 11 operating system(OS) and another using linux OS
- make sure these VM's are in the same resource group, region, and virtual network
- If they are not already running start them up

![rdc-window](https://github.com/user-attachments/assets/73af6d64-38a6-466c-bc38-37ba573f0251)

- When you created these VM's you were prompted to create a password for both
- Now we will be logging into our windows VM with the password created at the time of inception via RDP
- open the application Remote Desktop Connection and enter the public IP of our windows computer
- hit show options to fill in your username and then hit connect, prompting to fill in this VM's password as well.
- It may ask if you want to connect despite certificate errors, hit yes and it will sign you in

![wireshark-dwnld](https://github.com/user-attachments/assets/a3eab885-4480-4e50-a4ad-1b923294462d)

- Once signed in go to the browser and search wireshark
- go onto their site and download wireshark, we will use this program to monitor and inspect network traffic
- When the download is complete open up the settup and click next and install until getting to finish, which will complete the download and install

![start traffic](https://github.com/user-attachments/assets/38349401-03f3-4512-bae7-b85deaa556d2)

- After the installation open the application and start monitoring traffic by hitting the blue triangle shaped like a shark's fin
- Shortly after you will start seeing all network traffic

![icmp-ping](https://github.com/user-attachments/assets/bb4b2dc5-6799-43cb-bee4-8502e1923dce)

- Next we filer for all ICMP traffic by typing in icmp in the bar at the top of the page
- In order to see the traffic we have to cause activity by opening powershell and pinging our linux VM
- In order to ping or linux VM via powershell we just need to type "ping" and the private IP of the linux VM, in our case is 10.0.0.5
- Wireshark will show you the activity from your ping, which will be the VM's communicating with requests and replies

![image](https://github.com/user-attachments/assets/03820a90-8393-4052-b45e-19048ec2a65a)

- Now let's configure the firewall to deny our ping and monitor the activity from it
- In Azure go into linux VM's network settings and open the network security group by clicking it
- Block icmp protocol by adding a security rule to deny icmp
- make sure the priority is before all others to make this is the first rule the VM follows

![security in effect](https://github.com/user-attachments/assets/ee985b71-95ed-4338-9689-aa5e1b74d34c)

- Now that the firewall has been adjusted to deny icmp when we send request we should'nt get replies
- ping using the private IP again and this thime powershell will tell you request timed out
- Inside Wireshark you can see the Windows VM sending request to the linux VM but is recieving no replies

![security rule set](https://github.com/user-attachments/assets/6f83e1ac-a591-4276-8cc6-6e29c01a712b)

- If you want to recieve replies you just have to delete the rule created earlier

 Below will be several more examples of monitoring activity via wireshark, the only difference will be what kind of traffic is being filtered for


