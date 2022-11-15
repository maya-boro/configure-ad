<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Configure resources in Azure 
- Ensure connectivity between domain controller and the client 
- Install Active Directory
- In Active Directory, create an admin and a regular user account
- Connect the client to the domain controller 
- Setup Remote Desktop for non-administrative users on the client virtual machine

<h2>Deployment and Configuration Steps</h2>

<p>
Create a domain controller virtual machine with a base operating system of Windows Server 2022. Once created, make note of the resource group and virtual network (Vnet) that were created. After that, make the domain controller's NIC private IP address static. 
</p>
<p>
<img src="https://i.imgur.com/VouxADX.png" height="70%" width="70%" alt="Static IP Address"/>
</p>
<br />

<p>
Create the client virtual machine with a base operating system of Windows 10 and ensure that the client virtual machine is under the same resource group and virtual network (Vnet) as the domain controller virtual machine.
To confirm that both virtual machines are under the same virtual network (Vnet), you can check under Resource Visualizer.
</p>
<p>
<img src="https://i.imgur.com/R16AJ3J.png" height="70%" width="70%" alt="Resource Visualizer"/>
</p>
<br />

<p>
Log in to the client virtual machine with Remote Desktop Connection and open the Command Prompt to ping the domain controller’s private IP address with ping -t (IP address). You should observe a continuous ping.
</p>
<p>
<img src="https://i.imgur.com/Zcx0ewt.png" height="70%" width="70%" alt="Continuous Ping"/>
</p>
<br />

<p>
Log in to the domain controller virtual machine and enable ICMPv4 on the local Windows firewall.
</p>
<p>
<img src="https://i.imgur.com/TvzGDD4.png" height="70%" width="70%" alt="Enable ICMPv4"/>
</p>
<br />

<p>
Go back to the client virtual machine and check the command prompt. You should now observe a successful ping.
<p>
<img src="https://i.imgur.com/fmbAUAg.png" height="70%" width="70%" alt="Successful Ping"/>
</p>
<br />

<p>
Log in to the domain controller virtual machine and install Active Directory Domain Services.
</p>
<p>
<img src="https://i.imgur.com/5b07V1T.png" height="70%" width="70%" alt="AD Install"/>
</p>
<br />

<p>
Promote this server to a domain controller. Make sure to configure it as a new forest and write down the domain name.
</p>
<p>
<img src="https://i.imgur.com/MwY6Hjm.png" height="70%" width="70%" alt="Promote Server"/>
</p>
<br />

<p>
Allow for Active Directory to be installed. Once installation is complete, restart the domain controller virtual machine. 
Make sure to enter your user name as yourdomainnamehere.com\accountusernamehere when you log back into the domain controller. 
</p>
<p>
<img src="https://i.imgur.com/g6tQX3C.png" height="70%" width="70%" alt="Complete AD Install"/>
</p>
<br />

<p>
Create two Organizational Units (OU) with the names "_EMPLOYEES" and "_ADMINS" in Active Directory Users and Computers (ADUC).
</p>
<p>
<img src="https://i.imgur.com/BLEvF7l.png" height="70%" width="70%" alt="OU_Setup"/>
</p>
<br />

<p>
After setting up the two Organizational Units create a new employee. For tutorial purposes, you can name the employee "Jane Doe" with the username "jane_admin" and then add jane_admin to the "Domain Admins" Security Group. Once this is complete, close the Remote Desktop connection to the domain controller virtual machine and log back in as "mydomain.com\jane_admin"
</p>
<p>
<img src="https://i.imgur.com/GCfPVWQ.png" height="70%" width="70%" alt="New Admin"/>
</p>
<br />

<p>
Now go to the Azure portal and set the client’s virtual machine DNS settings to the domain controller’s private IP address. Once this is done, restart the client virtual machine within the Azure portal.
</p>
<p>
<img src="https://i.imgur.com/zHZ52EG.png" height="70%" width="70%" alt="DNS Settings"/>
</p>
<br />

<p>
Log in to the client’s virtual machine via Remote Desktop Connection as the original local admin and join the client to the domain. 
Please note that the computer will restart after this is completed.
</p>
<p>
<img src="https://i.imgur.com/IexuAJM.png" height="70%" width="70%" alt="Rename PC Advanced"/>
</p>
<br />

<p>
Log in to the client virtual machine under the admin account created previously during the organizational unit’s setup, open system properties, and click on "Remote Desktop." Allow "domain users" access to the remote desktop. Normal, non-administrative users can now log into the client virtual machine.
</p>
<p>
<img src="https://i.imgur.com/TIatFo2.png" height="70%" width="70%" alt="Remote Desktop Access"/>
</p>
<br />
