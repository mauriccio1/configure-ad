<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises AD(Active Directory) within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Setup resources in Azure
- Step 2 - Ensure connectivity between the <b> Client</b> and <b>Domain Controller</b>
- Step 3 - Install Active Directory
- Step 4 - Create an Admin and Normal User Account in AD
- Step 6 - Join Client-1 to your domain
- Step 7 - Setup Remote Desktop for non-administrative users on Client-1
- Step 8 - Create additional users and attempt to log into client-1 with one of the users


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/nR6ZWOp.png" height="80%" width="80%" alt="creating windows server"/>
</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”.
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time. <br/>
Create the Client VM (Windows 10) named “portf”. Use the same Resource Group and Vnet that was created.



</p>
<br />

<p>
<img src="https://i.imgur.com/Ka1LP2n.png" height="80%" width="80%" alt="static DNS"/>
</p>
<p>
Set Domain Controller’s NIC Private IP address to be static in the Azure portal.
</p>
<br />

<p>
<img src="https://i.imgur.com/nEPszXq.png" height="80%" width="80%" alt="AD domain services"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services<br/>



</p>
<br />
<p>
<ol> 
  <li> <img src="https://i.imgur.com/OgnTgrw.png" height="80%" width="80%" alt="promote to domain controller"/> </li>
<li> <img src="https://i.imgur.com/HE9Qf9j.png" height="80%" width="80%" alt="Add new forest"/>     </li>
</ol>
</p>
<p>
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is) <br>
Restart and then log back into DC-1 as user: mydomain.com/<i>username</i>
</p>
<br/>

<p>
<img src="https://i.imgur.com/J1OW9ob.png" height="80%" width="80%" alt="add directories"/>  
</p>
<p>

In "Active Directory Users and Computers" (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and “_ADMINS”.<br/>
Create a new employee ex. <i>jane_admin</i> with the same password used to login. <br/>
Add <i>jane_admin</i> to the “Domain Admins” Security Group.
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\<i>jane_admin</i>
Use the new user created as your admin account from now on.
  
</p>
<br/>

<p>
  <img src="https://i.imgur.com/eUzybNs.png" height="80%" width="80%" alt="set DNS server"/>  
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address<br/>
From the Azure Portal, restart Client-1<br/>


</p>
<br/>

<p>
  <ol>
<li>  <img src="https://i.imgur.com/Gxmrioi.png" height="80%" width="80%" alt="set client domain"/> </li>
<li> <img src="https://i.imgur.com/i7TBgAu.png" height="80%" width="80%" alt="sign in as admin"/>  </li>
  </ol>
</p>
<P>

  Login to Client-1 (Remote Desktop) as the original local admin and join it to the domain (computer will restart)

</P>
<br/>
<p>
 <img src="https://i.imgur.com/zB3FSPw.png" height="80%" width="80%" alt="verify computer appears"/>  
</p>
<p>

Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there.

</p>
<br/>

<p>
 <img src="https://i.imgur.com/JKsvqAu.png" height="80%" width="80%" alt="change RDP permission"/>  
</p>
<p>

Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

</p>
<br/>

<p>
  <ol>
<li>  <img src="https://i.imgur.com/l4Ap44G.png" height="80%" width="80%" alt="open powershell ISE"/> </li>
<li> <img src="https://i.imgur.com/AGdPaJb.png" height="80%" width="80%" alt="EMPLOYEES spawning"/>  </li>
</p>
<P>
Login to DC-1 as the new admin.<br/>
Open PowerShell_ise as an administrator and create a new file and paste the contents of the script into it. <br/>
Run the script and observe the accounts being created.  <a href=https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1>(script used)</a>


</p>
<br/>

<p>
 <img src="https://i.imgur.com/YdRcrzQ.png" height="80%" width="80%" alt="Sign in as bah.wufa"/>  
</p>
<p>

Attempt to log into Client-1 with one of the accounts.
Now any user in the active directory can login into the client.
</p>
<br/>
