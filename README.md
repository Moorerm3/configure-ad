<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1> Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/7b0c6e4b-312c-434c-b96c-ac3240b5c081)

Create a Resource Group called "Active-Directory-Lab", this will be the resource group used for *BOTH* "DC-1" & "client-1" VM.

![image](https://github.com/user-attachments/assets/61e2460f-f745-4f64-a5c7-f4c2f6cb4edd)
![image](https://github.com/user-attachments/assets/bd0ab472-cc1f-47a5-9f86-214c59b2465e)

While your virtual resource group is being deployed, create the virtual network "Active-Directory-VNet",  we will be using this for *BOTH* virtual machines to connect to. Make sure to add the resource group to this Virtual Network. *Always ensure regions match.*

![image](https://github.com/user-attachments/assets/fceabeb7-9fa0-4099-8813-ed454b9b837a)

Create a virtual machine called "DC-1", ENSURE to use the *IMAGE* Windows Server 2022 Datacenter: Azure Edition - x64 Gen2, because this will be our "Domain Controller".
 ENSURE to use a *SIZE* of at least 2 vcpus and 8 GiB memory.

![image](https://github.com/user-attachments/assets/0d32d636-9668-47ec-ba33-085f71621127)
![image](https://github.com/user-attachments/assets/34738de5-f91c-4d4c-abe2-b6cbd6f7cb7d)

ENSURE the VM on the resource group we created, is on the same Virtual Network we created earlier, we will use the same VNet for "client-1" VM.

![image](https://github.com/user-attachments/assets/44721b86-878a-40b7-927d-b71546523369)
![image](https://github.com/user-attachments/assets/db6d660c-496b-463a-843e-436e6efe8ef0)
![image](https://github.com/user-attachments/assets/cafd75b9-2d90-4736-bf7e-f10507a5fc95)

Create a VM called "client-1", we will be using the *IMAGE* Windows 10 Pro for "client-1". Add it to the "Active-Directory-Lab" Resource Group. Add it to the "Active-Directory-VNet" Virtual Network we created.

![image](https://github.com/user-attachments/assets/89a08c40-ae08-4368-ab03-d77272cd1656)
![image](https://github.com/user-attachments/assets/2138064c-fc96-407b-8771-a6e69cbf9c6d)

After VM is created, Set client-1's DNS settings to DC-1's Private IP Address, First we have to make sure the Private IP address of DC-1 wont change by setting the IP address from "Dynamic" to "Static". By clicking on DC-1's VM -> Network settings (left side) -> Click on the "Network Interface/IP Configuration" and set the IP configuration from Dynamic to Static, Press *SAVE*.

![image](https://github.com/user-attachments/assets/61ce3d78-ddb1-406d-a0c1-af5071a594c0)
![image](https://github.com/user-attachments/assets/c2f9a680-0387-4e72-a3c0-12961cbb51f8)
![image](https://github.com/user-attachments/assets/3271d936-80a7-46ff-b9ee-7d07064b4b39)
![image](https://github.com/user-attachments/assets/f6bc8e02-0a4b-418e-9150-5b1d02321585)
![image](https://github.com/user-attachments/assets/b3067e20-49a7-471b-80c8-9de01e56a73b)

Now that we set DC-1's Private IP address to static, we will copy the IP Address of DC-1 and go to client-1's->Network Settings-> Click on "Network Interface/IP Configuration"-> On the left side you will see "DNS Servers" setting-> Click Custom and Paste DC-1s Private IP Address from before. Click Save and you will see that Client-1 Inherited DC-1's Private IP address. Restart Client-1 for the effects to take place.

![image](https://github.com/user-attachments/assets/6d7090f4-0a24-4697-b41f-7a836bebacf0)
![image](https://github.com/user-attachments/assets/c1a1ab90-5ea7-4229-a18d-0681a4cdb694)
![image](https://github.com/user-attachments/assets/e3056966-cf84-4b2f-9270-6e4ede138fd0)

We will now copy the Public IP address for client-1 and paste it into RDP so we can confirm the changes through the command line by running "ping *DC-1's Private IP Address*". After confirming all packets got sent through, Run *ipconfig /all* and you can see at the bottom next to "DNS servers" the Private IP address of DC-1.


*PART 1 OF DEPLOYING ACTIVE DIRECTORY*
![image](https://github.com/user-attachments/assets/f8ca6166-7156-4216-bf49-a9427fffdee2)

In the right corner where you see the caution sign, click on it then click "Promote this server to a domain controller"

![image](https://github.com/user-attachments/assets/65c7a071-c867-4b03-ab5c-2c09a54f0435)
![image](https://github.com/user-attachments/assets/f2091ed1-65cb-421e-a0a3-8d0e50baad79)
![image](https://github.com/user-attachments/assets/55a43ed1-1a1e-4045-a9e7-3d77b87d0148)
![image](https://github.com/user-attachments/assets/54ed897d-4146-4771-a521-76063201f384)
![image](https://github.com/user-attachments/assets/4d8c6df3-97c7-4940-ae2d-9a8fdf1f25e4)

Click "Add roles and Features" -> "Add a new forest" -> and specify the Root domain name: *mydomain.com* -> For the Domain Controller Options for the (DSRM) password, type in "Password1" -> Uncheck *Create DNS Delegation* -> For Server Roles Check *Active Domain Directory Servers* Then click next Until you can Install.





































1
