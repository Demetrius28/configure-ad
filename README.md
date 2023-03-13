<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/HQdB4Qr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 1: Set up Resources in Azure
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
  - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
</p>
<br />

<p>
<img src="https://i.imgur.com/mNuAgOD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  Step 2: Check connectivity between Client and Domain Controller
  - Log in to the Domain Controller and enable ICMPv4 in on the local windows Firewall
  - Login to the Domain Controller in the Remote Desktop
  - Open Windows Defender Firewall
  - Select "Advanced Settings" on Left
  - Select "Inbound Rules"
  - Sort by "Protocol"
  - Enable ICMPv4 rules
</p>
<br />
<hr>

<img src="https://i.imgur.com/9aAxXeI.png" height="80%" width="80%" alt="Install AD"/>
</p>
<p>
Step 3. Install Active Directory

- Login to DC-1 through Remote Desktop
- Install Active Directory Domain Services: 
  - In the Server Manager, Select "Add Roles and Features"
  - Continue- Select Next, Next, Next, 
  - Select "Active Directory Domain Services"
  - "Add Features"; "Next"; "Next"; "Next"; "Install"; "Close"
  
 
  </p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/JqQMskv.png" height="50%" width="50%" alt="New Forest Set Up"/><img src="https://i.imgur.com/So3aEQ5.png" height="50%" width="50%" alt="Promote server to Domain Controller"/>
</p>
<p>
Step 4: Setting up Active Directory
  
  - Click the notification to select: "Promote this DC server to Domain Controller"
  - Click "Add new forest" 
  - Choose a password and take note of it
  - Complete installment- Next, Next, Next, Next, and Install
  - Allow the server to restart, which will log you out of the VM
  - Log back into DC-1 as user: mydomain.com\labuser
</p>
<br />
<hr>

<p>
<img src="https://i.imgur.com/8FrDbp0.png" height="80%" width="80%" alt="AD Set Up"/>
</p>
<p>
 Step 5. Creating Admin and Normal User Accounts in AD 
  
  - Click start menu and look up Active Directory Users and Computers (ADUC)
  - Create and take notes of names and passwords
    - an Organizational Unit (OU) called “_EMPLOYEES” 
    - a new OU named “_ADMINS”
    - a new employee named “Jane Doe” with the username of “jane_admin”
   - Select "Password never expires"

  </p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/cO8PUUn.png" height="80%" width="80%" alt="Add Admin"/>
</p>
<p>
  Step 6: Add Jane_admin to the "Domain Admins" Security Group
  
- Select "_ADMINS" and right click Jane Doe to select properties
- Select "Member of" 
- Add Domain Users: "Domain"
- Select "Check Names" to open name options
- Select "Domain Admins"
- Complete by Selecting "Ok"; "Ok"; "Apply"; "Ok"
- Close the Remote Desktop connection to DC-1
- Log back in as mydomain\jane_admin
</p>
<br />

<hr>

<p>
<img src="https://i.imgur.com/fmcc1ZY.png" height="40%" width="40%" alt="Join Client-1 to Domain"/><img src="https://i.imgur.com/d02jnLv.png" height="40%" width="40%" alt="Join Client-1 to Domain"/>
</p>
<p>
  Step 7: Join Client-1 to your domain (mydomain.com)

  - Go to Azure portal to set Client 1's DNS settings to DC-1's Private IP address
    - Open VM Client-1 
    - Select networking
    - Click the network interface link
    - Select DNS Servers on the left 
    - Choose "Custom" DNS Servers
    - Enter the DC's private ip address as the DNS server
    - From Azure, restart Client-1
  - Login to Client1 as labuser with Remote Desktop and join it to the domain
    - Right click start menu and selct "System" 
    - Select "Rename this PC (advanced)" and then Select "Change" 
    - In domain box type "mydomain.com" and select "OK" 
  - In Computer Name/Domain Changes box: 
    -"mydomain.com\jane.admin" with password
  - Select "OK" and restart when done
  - Login to DC-1 with Remote Desktop
  - Go to ADCU and verify Client-1 shows up in "Computers" on the root of the domain 
  
  
  </p>
<br />
<hr>
<p>
<img src="https://i.imgur.com/DYPq1RL.png" height="80%" width="80%" alt="Set up Non-Admin Users"/>
</p>
<p>
