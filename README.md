<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



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
</p>
<p>
In this lab, we will create two virtual machines within the same virtual network. One VM will function as the Domain Controller (DC), and the other will serve as a client machine. The Domain Controller will be assigned a static IP address to ensure consistent availability of Active Directory services. After configuring the DC, the client machine will be joined to the domain. We will then modify the client’s DNS settings so that it uses the Domain Controller as its DNS server.
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 must be configured with a static private IP address. Client-1 will connect to DC-1, and to verify connectivity, we will attempt to ping DC-1 from Client-1. Initially, the ping will fail. To resolve this, ICMPv4 must be enabled in the Windows Firewall on DC-1. Once ICMPv4 is allowed, Client-1 will be able to successfully ping DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Now we will log back into DC-1 to install Active Directory Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com" afterwards restart then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly you should be able to run Active Directory Users & Computers as shown below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Now we can begin creating Organizational Units (OUs). Start by creating an OU named _EMPLOYEES, followed by another OU named _ADMINS. To create an OU, right-click the domain, select New, then Organizational Unit, and complete the required field.  Next, create a user within the appropriate OU. Right-click inside the OU, select New, then User, and enter the user details. Create a user named Jane Doe, with the username Jane_admin, as she will serve in an administrative role. Finally, add Jane to the Domain Admins security group.
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com) from the Azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS. 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
To join Client-1 to the domain, navigate to your system settings and go to the About section. On the right, select Rename this PC (Advanced). From there, choose the option to change the domain and enter the domain name, mydomain.com. Next, provide your credentials in the format mydomain.com\labuser. Once this is completed, the computer will restart, and Client-1 will be successfully joined to mydomain.com.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 is now part of the domain. Next, we will configure Remote Desktop for non-administrative users on Client-1. Log into Client-1 as an administrator and open System Properties. Click on Remote Desktop and grant Domain Users access to Remote Desktop. Once these steps are completed, you should be able to log into Client-1 as a standard user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
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
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
