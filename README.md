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

- Step 1 Setup Resources In Azure
- Step 2 Ensure Connectivity 
- Step 3 Install AD
- Step 4 Create an Admin and Normal User Account in AD
- Step 5 Join Client-1 to your domain
- Step 6 Setup Remote Desktop for non-administrative users on Client-1
- Step 7 Create a bunch of additional users

<h2>Deployment and Configuration Steps</h2>

<p>
Step 1. Setup Resources in Azure

  <p>
  Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.
Ensure that both VMs are in the same Vnet.
<p>
<img width="1056" alt="Screenshot 2024-03-19 at 2 29 32 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/1f22b01b-0466-4213-90ad-d1a6cd72e7a5">


</p>
<p>
<p>
Step 2. Ensure Connectivity between the client and Domain Controller
  <p>
  Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 on the local Windows firewall
Check back at Client-1 to see the ping succeed.
<p>
<img width="1363" alt="Screenshot 2024-03-19 at 8 33 05 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/99f76700-81be-4807-ad45-8c0350087a35">
<p>
  <img width="994" alt="Screenshot 2024-03-19 at 8 43 32 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/826e5a22-bfad-4c25-a120-f9079d1a7917">

</p>

</p>
<p>
<p>
Step 3. Install Active Directory
  <p>
  Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com 
Restart and then log back into DC-1 as user: mrjumpan\labuser.
<p>
<img width="1440" alt="Screenshot 2024-03-19 at 8 49 35 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/a96df3f7-8497-4069-8040-51689acf1c51">
<p></p>
<img width="1316" alt="Screenshot 2024-03-19 at 8 53 22 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/1fd2fb7e-4ddf-454e-a962-7129c06dddc3">
<p>
<img width="988" alt="Screenshot 2024-03-19 at 9 46 42 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/0690a533-4730-494b-94dd-2b7e1955d292">

</p>


</p>
<p>
<p>
Step 4. Create an Admin and Normal User Account in AD
  <p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mrjumpan.com\jane_admin”
User jane_admin as your admin account from now on

<p>
<img width="757" alt="Screenshot 2024-03-19 at 9 14 17 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/a1349a5a-2572-447b-8c36-754385c956f5">
<p>
<img width="767" alt="Screenshot 2024-03-19 at 10 42 11 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/3b73ad57-08a2-401f-94a9-7180b9138a95">

</p>

</p>
<p>
<p>
Step 5. Join Client-1 to your domain (mrjumpman.com)
  <p>
  From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
<p>
<img width="728" alt="Screenshot 2024-03-19 at 10 01 52 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/26b72bc1-c2bf-40ae-bf8b-40870a1c92ff">
<p>
  <img width="412" alt="Screenshot 2024-03-19 at 10 13 23 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/16bcd04b-6b57-4c9e-808a-011e0ed793f6">
</p>
<img width="336" alt="Screenshot 2024-03-19 at 10 23 54 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/ff556943-6237-4830-beb7-6cf55e4005d8">

<p>
<p>
Step 6. Setup Remote Desktop for non-administrative users on Client-1
  <p>
  Log into Client-1 as mrjumpman.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with a Group Policy that allows you to change MANY systems at once.
<p>
<img width="996" alt="Screenshot 2024-03-19 at 10 35 48 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/66c9aa13-b3ec-4570-b9ca-fe268ea5d52e">


</p>
<p>
<p>
Step 7. Create a bunch of additional users
<p>
  Login to DC-1 as jane_admin
open PowerShell_ise as an administrator create a new file Run the script and observe the accounts being created. As you can see the users did not populate stay tuned for another lab trouble shooting the problem.
<p>
<<img width="1303" alt="Screenshot 2024-03-19 at 10 49 26 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/f0d2f2df-d0bf-4ad2-b14c-cea33b5b0b24">
<p></p>
<img width="1302" alt="Screenshot 2024-03-19 at 10 49 53 PM" src="https://github.com/Zakari23/configure-ad/assets/158666482/bdcc8b67-b3a5-4ef4-8bfe-17c0b7800072">

