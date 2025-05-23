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

Create a Resource Group called "*Active-Directory-Lab*". This will be the resource group used for both the "DC-1" and "*client-1*" VMs.

![image](https://github.com/user-attachments/assets/61e2460f-f745-4f64-a5c7-f4c2f6cb4edd)
![image](https://github.com/user-attachments/assets/bd0ab472-cc1f-47a5-9f86-214c59b2465e)

While your virtual resource group is being deployed, create the virtual network "Active-Directory-VNet",  we will use this for *BOTH* virtual machines to connect to. Make sure to add the resource group to this Virtual Network. *Always ensure regions match.*

![image](https://github.com/user-attachments/assets/fceabeb7-9fa0-4099-8813-ed454b9b837a)

Create a virtual machine called "DC-1", ENSURE to use the *IMAGE* Windows Server 2022 Datacenter: Azure Edition - x64 Gen2, because this will be our "Domain Controller".
 ENSURE to use a *SIZE* of at least 2 vCPUs and 8 GiB memory.

![image](https://github.com/user-attachments/assets/0d32d636-9668-47ec-ba33-085f71621127)
![image](https://github.com/user-attachments/assets/34738de5-f91c-4d4c-abe2-b6cbd6f7cb7d)

ENSURE the VM on the resource group we created is on the same Virtual Network we created earlier; we will use the same VNet for the "*client-1*" VM.

![image](https://github.com/user-attachments/assets/44721b86-878a-40b7-927d-b71546523369)
![image](https://github.com/user-attachments/assets/db6d660c-496b-463a-843e-436e6efe8ef0)
![image](https://github.com/user-attachments/assets/cafd75b9-2d90-4736-bf7e-f10507a5fc95)

Create a VM called "client-1", we will be using the *IMAGE* Windows 10 Pro for "client-1". Add it to the "Active-Directory-Lab" Resource Group. Add it to the "Active-Directory-VNet" Virtual Network we created.

![image](https://github.com/user-attachments/assets/89a08c40-ae08-4368-ab03-d77272cd1656)
![image](https://github.com/user-attachments/assets/2138064c-fc96-407b-8771-a6e69cbf9c6d)

After VM is created, Set client-1's DNS settings to DC-1's Private IP Address. First, we have to make sure the Private IP address of DC-1 won't change by setting the IP address from "Dynamic" to "Static". By clicking on DC-1's VM -> Network settings (left side) -> Click on the "Network Interface/IP Configuration" and set the IP configuration from Dynamic to Static, Press *SAVE*.

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

In the right corner where you see the caution sign, click on it, then click "Promote this server to a domain controller"

![image](https://github.com/user-attachments/assets/65c7a071-c867-4b03-ab5c-2c09a54f0435)
![image](https://github.com/user-attachments/assets/f2091ed1-65cb-421e-a0a3-8d0e50baad79)
![image](https://github.com/user-attachments/assets/55a43ed1-1a1e-4045-a9e7-3d77b87d0148)
![image](https://github.com/user-attachments/assets/54ed897d-4146-4771-a521-76063201f384)
![image](https://github.com/user-attachments/assets/4d8c6df3-97c7-4940-ae2d-9a8fdf1f25e4)

Click "Add roles and Features" -> "Add a new forest" -> and specify the Root domain name: *mydomain.com* -> For the Domain Controller Options for the (DSRM) password, type in "Password1" -> Uncheck *Create DNS Delegation* -> For Server Roles Check *Active Domain Directory Servers* Then click next Until you can Install.

![image](https://github.com/user-attachments/assets/56aeb4af-4170-4c87-910d-e045d124c2e4)

Logout of DC-1 and log back in as "*mydomain.com\labuser*"

![image](https://github.com/user-attachments/assets/3316235d-936c-4a0e-8f45-a6fca1eb1168)
![image](https://github.com/user-attachments/assets/db587984-b622-4bed-96fa-a57b13113ede)
![image](https://github.com/user-attachments/assets/b31f83b1-ac3b-45c0-840b-2d68bfa30562)
![image](https://github.com/user-attachments/assets/ea3709ba-1e20-4020-b5fc-4f9fab9f0602)

Open up Active Directory Users and Computers -> Under *mydomain.com* -> Create a new Organizational Unit called "*_EMPLOYEES*" -> Create a new Organizational Unit called "*_ADMINS*" -> Create a new user named "*Jane Doe*" under mydomain.com/_ADMINS -> for the user logon name, use "*jane_admin*"-> Create Password and only check "*Password never expires*".

![image](https://github.com/user-attachments/assets/063a19bb-7738-4258-afe6-79643bfffd3e)
![image](https://github.com/user-attachments/assets/06083bff-d788-4410-8486-e556caf812de)

Under Jane Doe Properties, go to "*Member Of*" -> Add -> Type in "*Domain Users*" -> "*Check Names*" -> and click "*OK*" -> *Apply*

![image](https://github.com/user-attachments/assets/7989313e-e211-404a-995a-393002a5b5a0)

Logout on DC-1 and log back in as "*mydomain.com\jane_admin*". Use *jane_admin* as your admin account from now on.

![image](https://github.com/user-attachments/assets/dac03a0c-ee88-497f-8862-ebdd21cf1754)
![image](https://github.com/user-attachments/assets/38c2eb77-fe20-4683-9136-6ee7d89fd749)
![image](https://github.com/user-attachments/assets/d58a5115-4b1f-4da6-a359-e42e1174ec2d)
![image](https://github.com/user-attachments/assets/85552785-d534-4b13-8d54-fb2b659c1145)

Log in as client 1 -> Right click start menu -> System -> Rename this PC (advanced) -> Click domain -> Type in "*mydomain.com*" -> Press OK -> a Prompt will show up asking for DC credentials -> Type in the credentials we just used to log into DC-1. You will get a prompt welcoming you to the domain and to restart your PC.

![image](https://github.com/user-attachments/assets/1aca91c7-d8e8-43af-bc59-ad1ac02b41b9)
![image](https://github.com/user-attachments/assets/7d860bde-7333-4b45-8051-8d60b7c25cc2)

Log into DC-1 and verify that Client-1 is on Active Directory Users and Computers by going to ADUC -> mydomain.com -> Computers and you will see client-1. Create a new Organizational Unit called "*_CLIENTS*" and click and drag client-1 into the new _CLIENTS OU.


*PART 2 OF DEPLOYING ACTIVE DIRECTORY: CREATING USERS WITH POWERSHELL*
![image](https://github.com/user-attachments/assets/2dba3c53-f468-42d6-8cd2-f9dd7ecfff48)
![image](https://github.com/user-attachments/assets/495a44a4-9dc1-40cb-acf6-061a59784394)
![image](https://github.com/user-attachments/assets/c280a6e2-87ec-4a95-b462-0d83e5facbff)
![image](https://github.com/user-attachments/assets/a32abc16-5222-4b6e-83e0-5053bd53f1ff)
![image](https://github.com/user-attachments/assets/46f63bb3-87ce-404d-98c4-2f2bb22e0054)

Setup Remote Desktop for non-administrative users on client-1. Log into Client-1 as mydomain.com\jane_admin -> Right Click start menu -> System -> Remote Desktop -> "Select users that can remotely access this PC" -> Add -> type "Domain Users" into the text box and press OK and OK.

![image](https://github.com/user-attachments/assets/bda32b0b-b901-4fe1-8ef2-aec18ccb1c65)

Open up Windows PowerShell ISE, *run as an administrator*, so we can run this script.

![image](https://github.com/user-attachments/assets/dfcf9310-cea0-4179-95ef-20f0770932aa)

Open up this [Script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) -> Copy Raw File

![image](https://github.com/user-attachments/assets/439fe713-d9f8-4bc7-9b51-af5646620255)
![image](https://github.com/user-attachments/assets/5b04ad8d-561b-493f-8286-41ac02015955)
![image](https://github.com/user-attachments/assets/fa4ee8e5-46df-4e6c-a7ab-67e31cc5f925)

In PowerShell ISE, create a new script and save it onto your desktop as "*create.users*". Paste the script into PowerShell and press the green start button at the top to let it run.

![image](https://github.com/user-attachments/assets/0e3a4594-fddf-445d-a69f-3cef830da424)
![image](https://github.com/user-attachments/assets/c1227da2-f0f8-467b-86f0-516885367bbc)

As this script is running, we can confirm its going into the "*_EMPLOYEES*" OU by going to ADUC in DC-1 and clicking on _EMPLOYEES. Log in as one of the users we created; the password of all the users is "*Password1*".


*PART 3 OF DEPLOYING ACTIVE DIRECTORY: GROUP POLICY & MANAGING ACCOUNTS*
![image](https://github.com/user-attachments/assets/58606624-a27c-4d52-b139-04c7e357e575)
![image](https://github.com/user-attachments/assets/3663433d-6f80-481c-8a11-f5b01cd35be8)
![image](https://github.com/user-attachments/assets/5e7f4731-ff1a-4060-b236-7caee01e0ce5)
![image](https://github.com/user-attachments/assets/1bceac56-b4b3-44ba-a3a5-c09bc1357b0b)

Get logged into DC-1 and run "*gp.msc*" -> In the drop down, go to *mydomain.com* and right click on "*Default Domain Policy*"-> Edit -> Computer Configuration -> Software Settings -> Windows Settings -> Name Resolution Policy -> Security Settings -> Account Policies -> Click Password Policy.

![image](https://github.com/user-attachments/assets/7178c652-186b-44f5-8a4c-bce9487bd7bc)
![image](https://github.com/user-attachments/assets/df13fba9-8890-46bd-ba69-bf138de28ae4)
![image](https://github.com/user-attachments/assets/f52b2d28-ba29-4511-9fa1-776844adf294)
![image](https://github.com/user-attachments/assets/f8775a17-5904-45a7-b6ae-ff2ca4e1d970)

Set the account lockout threshold to 5 invalid logon attempts, the account lockout duration to 30 minutes, and the account lockout counter to 10 minutes.

![image](https://github.com/user-attachments/assets/eb1633d1-d6c3-4632-a6bd-756955572d22)
![image](https://github.com/user-attachments/assets/906638dd-ac99-4a0f-9123-d58e5f161a8c)
![image](https://github.com/user-attachments/assets/6b60233f-712c-49b8-9d46-925672c445c0)

Log in to client-1 as *mydomain.com\jane_admin* and use CMD as an administrator and run "*gpupdate /force*", Log out of client-1.

![image](https://github.com/user-attachments/assets/f130697f-4e28-4b02-9291-a3e5fb49fbcf)
![image](https://github.com/user-attachments/assets/bb053245-7414-40d1-aa19-8ee026a1f0c7)

Log in Client-1 as "*mydomain.com\(any random user name that was created in the _EMPLOYEES OU)*", use the incorrect password 5 times until you see the security prompt.

![image](https://github.com/user-attachments/assets/89831f21-33de-42ff-a94c-7c6db511b2be)
![image](https://github.com/user-attachments/assets/2ef842ce-ea27-4fd7-bae0-9b865df38f0a)

Go into DC-1's ADUC (Active Directory Users & Computers), click mydomain.com -> _EMPLOYEES -> Locate the user's account we locked out, and you can see that there's a setting with a checkbox to unlock users. Unlock the user and apply.

![image](https://github.com/user-attachments/assets/415a40ad-6dca-4332-8ff3-8f43eaddd3a6)

We will be able to log in as "*bat.novo*" (or whatever user account you chose), to double-check check we logged in as the user, we can go to CMD-> and run "*whoami*". This confirms that we are on the bat.novo account.

![image](https://github.com/user-attachments/assets/a4c209e8-0ea6-47af-8a00-398b6538a0f8)
![image](https://github.com/user-attachments/assets/dc89b2a8-246c-40e8-ace3-ea9d7bf3bfeb)
To reset passwords within ADUC (Active Directory Users & Computers), we can go back to our _EMPLOYEES and right-click on a user. You can see the reset password and that will be the way to configure your users' password and password settings.

![image](https://github.com/user-attachments/assets/87a9b54e-da5c-4e8a-acfa-170c4e41c6ae)

Disabling an account goes through a similar process by right-clicking on a user and clicking "*Disable Account*". You can tell it's disabled from the little icon to the left showing a down arrow.

![image](https://github.com/user-attachments/assets/8a638c4d-be06-43de-803e-60e8df61d5af)
![image](https://github.com/user-attachments/assets/fd079a02-726e-40ee-b811-cf3629c8ccc5)
![image](https://github.com/user-attachments/assets/aadfd469-ea48-4fc1-966b-f1e85a4d7f0a)
![image](https://github.com/user-attachments/assets/779b9ca2-c762-49cc-96da-2ac928251817)

We will open up client-1, Run "*eventvwr.msc*" as administrator, a prompt screen will show up asking for the credentials for a domain admin, use *mydomain.com\jane_admin* as -> Custom Views -> Windows Logs -> Security -> Click the Find Action on the right side -> search "bat.novo" and look for the 5 Audit failures, these are the failures from when we put into the password wrong 5 times.
