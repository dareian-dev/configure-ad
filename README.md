<p align="center">
<img width="415" height="122" alt="Screenshot 2025-08-13 at 5 27 09 PM" src="https://github.com/user-attachments/assets/aa76ae4d-e7b3-427a-82ea-a9e5f70ced4b" />

</p>

<h1>Azure Active Directory </h1>
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

- Step 1 Set up Active Directory 
- Step 2 Join computer to the domain
- Step 3 Create Users &n Allow/Practice logging in as users

<h2>Deployment and Configuration Steps</h2>

<p>

  <img width="1322" height="465" alt="Screenshot 2025-08-16 at 8 37 39 PM" src="https://github.com/user-attachments/assets/ef96824d-5dec-4d14-88f3-6cd58acda734" />
  <img width="1470" height="950" alt="Screenshot 2025-08-17 at 1 23 49 PM" src="https://github.com/user-attachments/assets/817016c4-f790-4ebc-9995-60e30dc8518e" />


</p>
<p>
Step one you are going to create a resource group and name it Active Directory Lab. Next you will create a virtual machine named DC-1 (Domain Controller) and then set up the Virtual Network for that VM. After that you will create another virtual machine named client-1 which will interact with dc-1 but in order to ensure there are no connectivity issues you will have to set client-1’s DNS setting to dc-1’s private IP address. You will go to the dc-1 VM go to IP configurations in the settings and change the IP address settings from dynamic to static. Then log into dc-1 and change the firewall settings to off so you can test connectivity. Next you will go to client-1 VM, go to settings, DNS servers and change that to custom, and paste the private IP address of dc-1 so that client-1 can join the domain and anything searched will go through dc-1. Next you will open client-1 and ping dc-1’s private IP address to make sure you get a reply, and last you open powershell and run ipconfig /all and check the DNS server it should show dc-1’s private IP address.
</p>
<br />

<p>
<img width="1379" height="820" alt="Screenshot 2025-08-17 at 2 58 20 PM" src="https://github.com/user-attachments/assets/efbd4af6-a6e0-4246-92ed-85bca186f474" />

</p>
<p>
First thing you're going to do is go to dc-1 click start, server manager, then "under configure this local server" you’ll go to “add roles and features”, under server roles you’re going to select Active Directory domain services, and then click install.

Next promote dc-1 as a domain controller - configure Active Directory to become domain  controller aka new forest as.

Then in server manager on the dashboard youre going to click on the flag and click “promote this server to domain controller”- add a new forest and put mydomain.com as the title. Then keep clicking next until get to the install button…click that and it should automatically restart after its done installing.

You will need to sign-in again to dc-1 but since the VM is now Domain Server you need to specify whether you are a local user or a domain user so for the User name you will need to write mydomain.com\labuser 

Create domain admin user within the domain 
Open Active Directory users and computers and create organizational unit called _EMPLOYEES and create another one named _ADMINS
Create a new employee with any name you want “mary smith” with the username as mary_admin password Cyberlab123!

Add Mary smith to the domain admins security group. go to Mary smith in _admins right click and go to properties/ member of / and add to domain admins and then apply now Mary Smith is a domain admin. 

Next join client-1 to the domain 
To join to the domain log in as local labuser and then go to system/rename this pc(advanced) and connect it to the domain by selecting domain and typing mydomain.com to join, go to system and Remote Desktop and add domain users to enable all users to access and log into this computer
</p>
<img width="778" height="578" alt="Screenshot 2025-08-17 at 3 58 19 PM" src="https://github.com/user-attachments/assets/4234753e-ec45-439f-9de1-5ad04d8f289b" />

<br />

Creating users and Allow/Practice logging in as users

Log into dc-1 as admin and go to powershell ISE as administrator 

Create a new file and you will paste the script/code into it - the code will run and what it will do is generate 1000 random users. 

After the users are available in the OU _EMPLOYEES you can select any one of them. (Each user will have the same Password1) 
Now you can use any name inside of the _EMPLOYEES OU	and sign into client-1 with mydomain.com\(users name) 

<img width="1209" height="651" alt="Screenshot 2025-08-19 at 4 54 17 PM" src="https://github.com/user-attachments/assets/9506fcba-da7b-4122-84a5-2fb134832105" />

