<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>NSGs and Traffic Monitoring Between Azure VMs</h1>
In this tutorial, we look at the network traffic going to and from Azure Virtual Machines using Wireshark and also try out Network Security Groups. <br />




<h2>Environments and Technologies</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Command-Line Tools
- Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10
- Ubuntu Server

<h2>Actions and Observations</h2>

<p>
</p>
<p>
Welcome to my tutorial on Network Security Groups and checking network traffic. First, create two virtual machines (VMs) in Azure—one Linux and one Windows 10. Give each one two CPUs and make sure they are on the same virtual network (VNET).

Next, go to the Windows VM and download Wireshark from this link: https://www.wireshark.org/download.html. Install and open Wireshark, then set it to filter for ICMP traffic only.

ICMP is a type of message used to check if devices can connect. The "ping" command uses ICMP to test if one machine can reach another. When you filter for ICMP in Wireshark and ping the private IP of the Linux VM, you'll see the ping packets appear in Wireshark. 
</p>
<br />
<p>
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can look at each packet closely to see the exact data being sent with each ping. The picture below shows an example of this. 
</p>
<br />
<p>
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the next part of the lab, we will use the command ping -t on the Windows machine to continuously ping the Linux machine. This will keep sending pings until we stop it.

While the pings are running, go to the Linux machine and block incoming ICMP traffic using its firewall. After blocking it, the Windows machine will stop getting ping replies (echo replies) from the Linux machine.

To block ICMP, we’ll create a new Network Security Group (NSG) for the Linux machine and set it to block ICMP traffic. If we want to allow pings again later, we can go to the Linux machine’s NSG settings in Azure and allow ICMP traffic. 
</p>
<br />
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Next, we’ll use the Windows machine to connect to the Linux machine using SSH. SSH doesn’t have a graphical interface—it just gives you access to the Linux command line.

Before connecting, set the Wireshark filter to capture only SSH packets. Then, use the command ssh labuser@10.0.0.5 in Command Prompt to start the SSH connection. As soon as the connection begins, you’ll see Wireshark start capturing SSH traffic.
</p>
<br />
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we’ll use Wireshark to filter for DHCP traffic. DHCP (Dynamic Host Configuration Protocol) runs on ports 67 and 68 and is used to assign IP addresses to devices.

To see it in action, set the Wireshark filter to capture DHCP packets. Then, on the Windows machine, run the command ipconfig /renew to request a new IP address. After running the command, you’ll see DHCP traffic appear in Wireshark.
</p>
<br />
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now it’s time to filter DNS traffic. Set Wireshark to capture only DNS packets.

To generate DNS traffic, run the command nslookup www.google.com on the Windows machine. This command asks the DNS server for the IP address of Google. When you do this, Wireshark will show the DNS request and response.
</p>
<br />
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
