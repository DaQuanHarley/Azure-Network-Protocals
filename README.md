<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this guide, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic



<h2>Actions and Observations</h2>


Create a resource group so we can put both of our virtual machines in it. Then, make our first virtual machine. The first virtual machine we are going to make is a Windows 10 VM. Select the resource you made, and then name the virtual machine. Make sure you select Windows 10 Pro, version 22H, as the operating system. As for the size of the machine, we are going to want at least 2 vCPUs. Create a username and password of your choice, and keep the inbound port rules as the default options.

<p>
<img src="https://i.imgur.com/Dya86IN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  

<p>
<img src="https://i.imgur.com/hVk1u5I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


<p>
<img src="https://i.imgur.com/f0rOpnT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Click on next until we get to the networking page, and it should automatically create a virtual network and subnet for us. 
  

<p>
<img src="https://i.imgur.com/4VdVhbl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click review and create our VM.
  
Now that we have created our first VM, we are going to go ahead and create our second VM, but this time it will be a Ubuntu Server 20.04 LTS machine. It will be the same process as creating our first machine, but instead, we are going to switch the SSH public key to a password. 
  
<p>
<img src="https://i.imgur.com/r1QTnVW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
<p>
<img src="https://i.imgur.com/1QDqZlv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click next until we get to the networking page again.
  
  The networking should automatically give us the virtual network from the Windows VM as well as the subnet. 
  
<p>
<img src="https://i.imgur.com/Ftv2ZsC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 Click review and create, and it will create our second VM.
 
Connect to our Windows 10 VM using the Remote Desktop Connection app. Once we are connected, go to the browser download and install Wireshark.
 
 "Wireshark is a free and open-source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education." 
 
Open Wireshark, highlight Ethernet then click on the shark fin, and filter for ICMP traffic only.
 
 <p>
<img src="https://i.imgur.com/MlRcsSy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<img src="https://i.imgur.com/DXBDi2D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
We are going to want to retrieve the private IP address of our Ubuntu VM and then attempt to ping it from within our Windows 10 VM using Wireshark. To ping the private IP address of the Ubuntu machine, open CMD or Powershell on the Windows machine and type: ping 10.0.0.5 or whatever the private IP address is for your Ubuntu machine.
 
<p>
<img src="https://i.imgur.com/aKKd25e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://i.imgur.com/gZWSoRt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
Now we are going to initiate a non-stop ping from our Windows 10 VM to our Ubuntu VM.
 
Open the Network Security Group of our Ubuntu machine and disable incoming (inbound) ICMP traffic. To disable incoming ICMP traffic, click "Add" a new rule and copy everything exactly from the picture. Once that is done, you can create the rule, which will automatically create and show up as a new one.
 
 <p>
<img src="https://i.imgur.com/s6vZovU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://i.imgur.com/GN4Brwz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/uONOeMv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 Now that we have disabled incoming ICMP traffic from the Linux VM, the ping request will time out if we go back to the Windows VM. 
 
Re-enable ICMP traffic for the Network Security Group that your Ubuntu VM is using
Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity (should start working)
Stop the ping activity

 
<h2>Observe SSH Traffic</h2>
 Back in Wireshark, start a packet capture and filter for SSH traffic only
 
 <p>
<img src="https://i.imgur.com/P5rwTFd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

From the Windows 10 VM, “SSH" into the Ubuntu Virtual Machine (via its private IP address).
</p>
<p>
Open PowerShell, and type: ssh labuser@[private IP address].
</p>
 <p>
<img src="https://i.imgur.com/bTrgkF6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Powershell/CMD should ask "Are you sure you want to continue connecting (yes/no/[fingerprint])?" type "yes"
</p>
 <p>
  Type the password used for labuser from the Ubuntu VM (a.k.a Linux VM)
</p>

 <p>
<img src="https://i.imgur.com/vvuEIeh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Exit the SSH connection by typing 'exit' and pressing [Enter]
 <p>
<img src="https://i.imgur.com/vsHqpls.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  
<h2>Observe DHCP Traffic</h2>

 Back in Wireshark, filter for DHCP traffic only
 <p>
<img src="https://i.imgur.com/OdV7daQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the Windows 10 VM, attempt to issue the VM a new IP address from the command line
 <p>
<p>
  Open PowerShell as an admin and run: ipconfig /renew
  <p/>
<img src="https://i.imgur.com/dQC0GYr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Observe the DHCP traffic appearing in Wireshark
 <p>
<img src="https://i.imgur.com/0K8HnLi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h2>$\color{Red}\Huge{\textbf{Lab Cleanup (IF ONLY USED FOR TEST PURPOSES)}}$</h2>

 <p>Close your Remote Desktop connection</p>

 Delete the Resource Group(s) created at the beginning of this lab
 
 Verify Resource Group Deletion
 
 
 
 
 
  
  
