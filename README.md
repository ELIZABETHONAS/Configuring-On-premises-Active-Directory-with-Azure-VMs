# Configuring-On-premises-Active-Directory-with-Azure-VMs
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This project outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- 
- 
- 
- 

<h2>Deployment and Configuration Steps</h2>

<p>
  
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.26.54%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%205.29.14%20PM.jpeg?raw=true)

</p>
<p>

  -  Setup Resources in Azure, for this project that will be "ADLAB"
     -  Create the Domain Controller VM (Windows Server 2022) named “DC1”
     -  Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
     -  Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group (ADLAB )and Vnet that was created in Step 1.a
</p>
<br />

<p>
  
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%205.25.30%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.41.30%20PM.jpeg?raw=true)
</p>
<p>

  -   Set Domain Controller’s NIC Private IP address to be static
      -  Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
</p>
<br />

<p>
  
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-04%20at%204.30.20%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.45.36%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.49.48%20PM.jpeg?raw=true)  
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.53.43%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.54.19%20PM.jpeg?raw=true)
</p>
<p>

  -   Ensure Connectivity between the client and Domain Controller, through the below steps:
      - Login to Client-1 with Remote Desktop and ping DC1’s private IP address with ping -t <ip address> (perpetual ping)
      - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
      - Check back at Client-1 to see the ping succeed

</p>
<br />

<p>
  
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.50.53%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.57.12%20PM.jpeg?raw=true)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%204.24.40%20PM.jpeg)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%204.29.23%20PM.jpeg)
![thisismyimage](https://github.com/ELIZABETHONAS/Configuring-On-premises-Active-Directory-with-Azure-VMs/blob/main/Screenshot%202023-06-03%20at%203.45.36%20PM.jpeg?raw=true))
</p>
<p>
  
   -  Install Active Directory
      - Login to DC-1 and install Active Directory Domain Services
      - Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
      - Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />

<p>
  
![thisismyimage]()
</p>
<p>

  -   Create an Admin and Normal User Account in AD
      - In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
      - Create a new OU named “_ADMINS”
      - Create a new employee named “Elizabeth.Onas” (same password) with the username of “Elizabeth.Onas”
      - Add Elizabeth.Onas to the “Domain Admins” Security Group
      - Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\Elizabeth.Onas”
      - User Elizabeth.Onas as your admin account from now on

</p>
<br />

<p>
  
![thisismyimage]()
</p>
<p>

 -   Join Client-1 to your domain (mydomain.com)
      - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
      - From the Azure Portal, restart Client-1
      - Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
      - Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers”                  container on the root of the domain
      - Create a new OU named “_CLIENTS” and drag Client-1 into there

</p>
<br />

<p>
  
![thisismyimage]()
</p>
<p>

 -   Setup Remote Desktop for non-administrative users on Client-1
     - Log into Client-1 as mydomain.com\jane_admin and open system properties
     - Click “Remote Desktop”
     - Allow “domain users” access to remote desktop
     - Now log into Client-1 as a normal, non-administrative user now

</p>
<br />

<p>
  
![thisismyimage]()
</p>
<p>

 -   Create a bunch of additional users and attempt to log into client-1 with one of the users
     - Login to DC-1 as elizabeth.onas
     - Open PowerShell_ise as an administrator
     - Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
     - Run the script and observe the accounts being created
     - When finished, open ADUC and observe the accounts in the appropriate OU
     - The End

</p>
<br />
