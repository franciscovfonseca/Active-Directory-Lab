<p align="center">
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/3a1e74a2-4d7a-4395-9646-97f107b0ae4e" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>


<h2>Description</h2>
In this lab, we'll create two VMs in the same VNET - one as a Domain Controller (DC) with a static IP offering Active Directory services, and the other as a Client machine.
</p>
The client will join the domain, and its DNS settings will be configured to use the DC as the primary DNS server.

The project involves establishing an Active Directory system for centralized user credential oversight and network traffic management.

By routing all internet traffic through the main server (Active Directory) via organization devices (Clients), administrators can monitor network activity and detect suspicious logs.

Additionally, the project includes a PowerShell script to generate 1,000 users and showcases a device within the organization's domain, ensuring efficient management of user credentials and network traffic.
<br />
<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<br />

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<br />

<h2>Project Diagram</h2>

<p align="center">
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/515cdb6d-7aa1-4806-b4a5-77ae9893bedb" height="50%" width="50%" alt="9"/><br />
</p>
<br />

<h2>Step 1: Setup</h2>


First, using Azure, create a Resource Group.<br>
Then, create 2 Virtual Machines (VMs).<br>

One will be the **Domain Controller** and the other will be the **Client**.<br>

To create the Domain Controller, give the VM a name as well as assign it to the Resource Group created before.


<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/456a1394-bd3d-4ce8-945a-c1e23e4d897d" height="70%" width="70%" alt="9"/><br />
<br />


Now for the Image use **Windows Server 2022**.<br>
It is recommended for the size to use 4 vcpus.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8e18ab36-9543-4327-9f55-892fc6f599b1" height="70%" width="70%" alt="9"/><br />
<br />

Give the admin log in credentials that can be remembered or just write them down in notepad.<br>
Then, click *Next* until reaching the **Networking** tab.<br>

⚠️ Take note of the Virtual Network created: This will be important when creating the Client VM.<br>

Check the box under Licensing then ***Review and Create*** the VM.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a5dbd18a-2b7a-433e-b101-07302f503a49" height="70%" width="70%" alt="9"/><br />
<br />

Now, create the Client VM.<br>
Same thing as the first one except the image should be using **Windows 10**.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d9d106da-540a-487f-8cfa-97bf3955945d" height="70%" width="70%" alt="9"/><br />
<br />

Click *Next* until reaching the **Networking tab**.<br>

➡️ Make sure the Virtual Network is the same as the one for the Domain Controller.<br>

Finally ***Review and Create***.

<h2></h2>
<br />

Now it's time to set the Domain Controller's NIC Private IP to **Static**<br>

Go to the Domain Controller and click on the **Networking** tab.<br>
After that, click on the *Network Interface*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e702cc27-b53c-42af-b002-3a7ca8322992" height="60%" width="60%" alt="9"/><br />
<br />

Now, go the **IP configurations** tab and click on the IP configuration. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/2d248eb4-13d7-42de-9945-1aa600fdae1d" height="60%" width="60%" alt="9"/><br />
<br />

Now, change the *Allocation* from **Dynamic** to **Static**.<br>
Then click ***Save***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8d3b0db5-68cc-4626-a552-f62aac7574a7" height="60%" width="60%" alt="9"/><br />
<br />
<h2></h2>
<br />

After that, using the **User** and **Password** created before, login to the Client with its IP address in **Remote Desktop Connection**. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/3b7d7c7d-c71f-4056-bccd-3999bdd4dc3d" height="55%" width="55%" alt="9"/><br />
<br>

Then, using **Command Prompt**, ping the Domain Controller with its Private IP Address.<br>
Type in ***ping (Your DC Private IP) -t*** to perpetually ping.<br>

For now it will time out.

```commandline
ping 10.0.0.4 -t
```


<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a9ac0069-9240-42ed-b61f-5606a900ec44" height="80%" width="80%" alt="9"/><br />
<br>

Following this, its time to enable **ICMPv4**.<br>

First, login to the Domain Controller VM then open ***Windows Defender Firewall with Advanced Security***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d42ded65-aee3-4e87-bc61-a7b2b3050e50" height="85%" width="85%" alt="9"/><br />
<br>

Click on **Inbound Rules** and Sort by **Protocol**.<br>
Look for the rules with ***Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)***.<br>

There will be two of them *(Both at the bottom of the image below)*

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/67f09539-fc53-451f-82dc-cc0cd9a4d77d" height="80%" width="80%" alt="9"/><br />
<br>

Right-click and **Enable** both rules.<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/83a47889-6fc4-40ec-aa6d-082ea4620238" height="55%" width="55%" alt="9"/><br />
<br>

Now go back to the **Client VM** and check on the *Command Prompt*.<br>
This time it should be properly pinging the Domain Controller.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/51bbc494-db1e-41c1-9f4c-6a84d55585b0" height="80%" width="80%" alt="9"/><br />
<br>
<br>

<h2>Step 2: Installing Active Directory</h2>

Now it's time to install **Active Directory**.<br>
Go to the Domain Controller and in *Server Manager* click on *Add roles and features*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5c47459b-596d-4b61-a7ba-820a37bc50e8" height="80%" width="80%" alt="9"/><br />
<br>

Click *Next* until reaching the *Server Roles* section.<br>
Check the box next to *Active Directory Domain Services* then *Add Features*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5a62a3a5-9570-4eef-af9f-36f97764d09f" height="80%" width="80%" alt="9"/><br />
<br>

Click *Next* until reaching the *Confirmation* tab then click *Install*.<br>
It may take a while to install.<br>
<br>
Once it says =*Configuration required. Installation succeeded on (**Your DC name here**)*= 🡪 Click *Close*<br>
<br>
Towards the top-right corner of the *Server Manage*r window, there will be a flag and a yellow triangle with a ⚠️ symbol 🡪 Click on that.<br>
Then click on "*Promote the server to a domain controller*".

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a067d615-f065-479a-97ef-65a23609406d" height="40%" width="40%" alt="9"/><br />
<br>

A window will pop up for a **Configuration Wizard**.<br>
Check the bubble ◉ next to "*Add a new forest*"<br>
<br>
After that give it a domain name (example in the image below) and then click *Next*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e1bfed73-b689-4cbf-a950-56f6ee5341e9" height="80%" width="80%" alt="9"/><br />
<br>

Give it a DSRM password (Required but wont be used in this tutorial) Click next.

<img src="https://i.imgur.com/TYXfTrJ.png" height="80%" width="80%" alt="9"/><br />

Next, the NETBIOS domain will be made. This may take a moment. Once it is made, Click next until reaching the "Prerequisites Check" tab. This process will take a moment. Now click "Install"

After Installing the VM will reboot. Once it is rebooted, Log back into the Domain Controller with the domain name and the username. Example below.

<img src="https://i.imgur.com/nT5uFiT.png" height="55%" width="55%" alt="9"/><br />

<h2>Step 3: Creating a Domain Admin</h2>

Once logged in, using Server Manager click on tools in the top-right corner. Next click on "Active Directory Users and Computers."

<img src="https://i.imgur.com/grdGvPg.png" height="70%" width="70%" alt="9"/><br />

In the Domain container, create a new "Organizational Unit"

<img src="https://i.imgur.com/9DGmBMS.png" height="80%" width="80%" alt="9"/><br />

Name the OU "_ADMINS", then click OK. In the "_ADMINS" tab, create a new "User"

<img src="https://i.imgur.com/gUev6rr.png" height="80%" width="80%" alt="9"/><br />

Name this anything. Just remember the user and password. Uncheck the box that is next to "User must change password at next logon." This wont be necessary. Click next then click Finish.

<img src="https://i.imgur.com/fmMKhNj.png" height="50%" width="50%" alt="9"/><br />
<img src="https://i.imgur.com/S0c7T05.png" height="50%" width="50%" alt="9"/><br />

Now add this user to the "Domain Admins" security group. Right-click on the user create, then click "Properties." Click on the "Members of" tab, then click "Add." 

<img src="https://i.imgur.com/3MooGCr.png" height="50%" width="50%" alt="9"/><br />

Type "domain" in the box under "Enter the object names to select:" then click "Check Names" 

<img src="https://i.imgur.com/WnHnpsK.png" height="60%" width="60%" alt="9"/><br />

Choose the "Domain Admins" option then click OK

<img src="https://i.imgur.com/eHOKSWT.png" height="80%" width="80%" alt="9"/><br />

Now, click "Apply." The user has successfully been added to the Domain Admins security group. Click OK. Now logout of the Domain controller and re-log as the user just created.

<img src="https://i.imgur.com/oECi1Rd.png" height="50%" width="50%" alt="9"/><br />

<h2>Step 4: Setting Client DNS Settings to Domain Controller Private IP Address</h2>

First, on Azure go to the Client VM. Next, go to the Networking tab and click on the Network Interface.

<img src="https://i.imgur.com/0jFQlMM.png" height="60%" width="60%" alt="9"/><br />

Next, go to the "DNS Servers tab and create a custom DNS Server. Add a custom server using the Domain Controller's Private IP address. Example Below.

<img src="https://i.imgur.com/DK1mOBp.png" height="60%" width="60%" alt="9"/><br />

Now click "Save" Next go back to the Client and click "Restart in the "Overview" tab 

<img src="https://i.imgur.com/Cq7d1PZ.png" height="80%" width="80%" alt="9"/><br />

Once the Client is restarted, login to the client with Remote Desktop as the admin account created.

<img src="https://i.imgur.com/LkqUK6Q.png" height="50%" width="50%" alt="9"/><br />

Once logged in go to Settings>System>About and click on "Rename this PC(advanced)"

<img src="https://i.imgur.com/1tu4Kwj.png" height="80%" width="80%" alt="9"/><br />

Now Click on "Change..."

<img src="https://i.imgur.com/zKDbFIs.png" height="50%" width="50%" alt="9"/><br />

Now check the bubble next to "Domain" then type in the domain name (Your own domain name). There should be window that pops up for a login. Use the admin previously created to login. Example below:

<img src="https://i.imgur.com/nT3wd3q.png" height="80%" width="80%" alt="9"/><br />
<img src="https://i.imgur.com/oVmTUG5.png" height="40%" width="40%" alt="9"/><br />

Success. The VM will now restart after a short period.

<h2>Step 5: Setting up Remote Connection for Domain Users</h2>

Now, log into the Domain Controller. Go back to Server Manager>Tools>Active Directory Users and Computers. Under the Domain container, go to the "Computers" tab. It should show that the client has been added to the list.

<img src="https://i.imgur.com/TT1JXxR.png" height="80%" width="80%" alt="9"/><br />

Now, log into the Client as the admin user created and go to System Settings>Remote Desktop. Click on "Select users that can remotely access this PC" Next click Add.

<img src="https://i.imgur.com/oKxoprK.png" height="80%" width="80%" alt="9"/><br />

In the box at the bottom, type in "Domain Users" and Check Names. Next click OK.

<img src="https://i.imgur.com/JXijlI7.png" height="60%" width="60%" alt="9"/><br />

<h2>Step 6: Creating Domain Users</h2>

In the Domain Controller, open "Windows PowerShell ISE." Make sure to open it as Administrator. Click "New File" in the top left corner.

<img src="https://i.imgur.com/I3165Lu.png" height="85%" width="85%" alt="9"/><br />
<img src="https://i.imgur.com/Y5BAh4S.png" height="80%" width="80%" alt="9"/><br />

Next, copy and paste the script from this link into the text editor. 

https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

Choose "1_CREATE_USERS.ps1".

<img src="https://i.imgur.com/m6N6m4j.png" height="80%" width="80%" alt="9"/><br />

Run "Set-ExecutionPolicy Unrestricted" in the command line.

```commandline
Set-ExecutionPolicy Unrestricted
```

<img src="https://i.imgur.com/xCzIjyZ.png" height="65%" width="65%" alt="9"/><br />

Change directory to script.

```commandline
cd C:\Users\tsmith\Desktop\1_CREATE_USERS.psy
```

Now, click the Run button to run the script. This will start creating domain users with usernames and passwords (The Password for these users will be "Password1") Example below:

<img src="https://i.imgur.com/IN8xvda.png" height="65%" width="65%" alt="9"/><br />
<img src="https://i.imgur.com/RMyC0Co.png" height="80%" width="80%" alt="9"/><br />

Go to Server Manager>Tools>Active Directory Users and Computers. Under the "_EMPLOYEES" tab, look at all of the users created from the script.

<img src="https://i.imgur.com/f2xPlao.png" height="80%" width="80%" alt="9"/><br />

These names are all randomly generated. Choose one and log into the Client VM with the username it is assigned. (Remember the password is "Password1)

<img src="https://i.imgur.com/LoWC3Er.png" height="50%" width="50%" alt="9"/><br />

Congrats! You completed this tutorial.

<h2>Conclusion</h2>

Active Directory is crucial for organizations to effectively control their network traffic and prevent unauthorized access to internal networks or leakage of information to external parties. Understanding and learning about Active Directory is a fundamental principle for all IT professionals, regardless of their specific roles. Hope you found this blog both informative and valuable.

