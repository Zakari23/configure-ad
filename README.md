# Configure-AD
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

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
Step 1. Setup Resources in Azure

  <p>
  Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 2. Ensure Connectivity between the client and Domain Controller
  <p>
  Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 on the local Windows firewall
Check back at Client-1 to see the ping succeed.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 3. Install Active Directory
  <p>
  Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 4. Create an Admin and Normal User Account in AD
  <p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on

<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 5. Join Client-1 to your domain (mydomain.com)
  <p>
  From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 6. Setup Remote Desktop for non-administrative users on Client-1
  <p>
  Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with a Group Policy that allows you to change MANY systems at once.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
Step 7. Create a bunch of additional users
  <p>
  Login to DC-1 as jane_admin
open PowerShell_ise as an administrator create a new file Run the script and observe the accounts being created.
<p>
<img width="1046" alt="1" src="https://github.com/Zakari23/post-install-config/assets/158666482/b93b07ef-9e53-4f3d-b38e-4a967ad58119">

</p>
<p>
<p>
