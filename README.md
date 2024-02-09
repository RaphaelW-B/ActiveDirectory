<div align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</div>

<h1>Deploying On-premises Active Directory in the Cloud (Azure)</h1>
This guide illustrates the process of implementing on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Utilized</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Utilized</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<p>
In this lab, we will create two VMs within the same VNET. One will function as a Domain Controller, while the other will serve as a Client machine. We will assign a static IP to the DC since it will provide Active Directory services to the client machine. The client machine will be joined to the domain, and we will configure its DNS settings to use the DC as its DNS server.
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 requires a static Private IP Address. Client-1 will establish connectivity with DC-1; initially, ping might fail. We'll enable ICMPv4 on the firewall of DC-1, enabling successful pings from Client-1 to DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
We'll then log back into DC-1 to install AD Users & Computers, promote the VM to DC, establish a new forest as "mydomain.com," and restart. Upon logging back in as user: "mydomain.com\labuser," if executed correctly, AD Users & Computers should be accessible.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
We can now proceed to create Organizational Units (OUs). Firstly, let's create an OU named _EMPLOYEES and another named _ADMINS. To do so, right-click on the domain area, select new->Organizational Unit, fill out the fields, then click inside your OU, right-click, select new, and choose user to input information for a new user, "Jane Doe," who will be an Admin, with the username Jane_admin. Finally, add Jane to the domain admins security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
Moving forward, Jane_admin will serve as the administrator account. Next, we'll join Client-1 to the domain (mydomain.com). From the Azure portal, we'll adjust Client-1's DNS settings to the DC's Private IP address, then restart Client-1 to ensure it's utilizing DC-1 as its DNS.
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
Next, we'll proceed to join Client-1 to the domain. Navigate to system settings, select "About," then choose "Rename this PC (advanced)." Opt to change the domain to "mydomain.com," and enter credentials from mydomain.com\labuser. After a computer restart, Client-1 will be integrated into mydomain.com.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 is now part of the domain. We'll now set up remote desktop for non-administrative users on Client-1. Log into Client-1 as an admin, open System Properties, click on "Remote Desktop," and grant "domain users" access. Following these steps, normal users should be able to log into Client-1.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, to verify that normal users can RDP into Client-1, we'll employ a script to generate thousands of users into the domain using PowerShell. Once users are created, we'll select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
The Powershell script created a user with the username "bab.hubo." We successfully logged into Client-1 with these credentials as a normal user.
</p>
