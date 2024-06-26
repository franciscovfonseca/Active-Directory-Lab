<p align="center">
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/3a1e74a2-4d7a-4395-9646-97f107b0ae4e" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>


<h2>Description</h2>

In this lab, we'll create two **Virtual Machines** in the same Virtual Network:
- One as a **Domain Controller (DC)** with a static IP offering *Active Directory* services.
- And the other one as a **Client** machine.

The Client will join the domain, and its DNS settings will be configured to use the DC as the primary DNS server.

The project involves establishing an **Active Directory** system for centralized user credential oversight and **Network Traffic Management**.

By routing all internet traffic through the main server (Active Directory) via organization devices (Clients), administrators can monitor network activity and detect suspicious logs.

Additionally, the project includes a **PowerShell** script to generate 1,000 users and showcases a device within the organization's domain, ensuring efficient management of user credentials and network traffic.
<br />
<br />


<h2>Environments and Technologies Used</h2>

  🔹 Microsoft Azure (Virtual Machines/Compute)<br>
  
  🔹 Remote Desktop<br>
  
  🔹 Active Directory Domain Services<br>
  
  🔹 PowerShell<br>

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


First, using Azure, create a **Resource Group**.

Then, create 2 **Virtual Machines** (VMs).

One will be the ***Domain Controller*** and the other will be the ***Client***.

To create the Domain Controller, give the VM a name as well as assign it to the Resource Group created before.


<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/456a1394-bd3d-4ce8-945a-c1e23e4d897d" height="70%" width="70%" alt="9"/><br />
<br />


Now for the Image: use **Windows Server 2022**.

Make sure to select at least *2 vcpus and 16 GiB memory*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8e18ab36-9543-4327-9f55-892fc6f599b1" height="70%" width="70%" alt="9"/><br />
<br />

Give the admin log in credentials that can be remembered or just write them down in notepad.

Then, click *Next* until reaching the **Networking** tab.<br>

⚠️ Take note of the Virtual Network created: This will be important when creating the Client VM.<br>

Check the box under Licensing then ***Review and Create*** the VM.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a5dbd18a-2b7a-433e-b101-07302f503a49" height="70%" width="70%" alt="9"/><br />
<br />

Now, create the Client VM.

Same thing as the first one except the image should be using **Windows 10**.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d9d106da-540a-487f-8cfa-97bf3955945d" height="70%" width="70%" alt="9"/><br />
<br />

Click *Next* until reaching the **Networking tab**.<br>

➡️ Make sure the **Resource Group** and the **Virtual Network** are the same as the one for the *Domain Controller*.<br>

Finally ***Review and Create***.


<h2></h2>
<br />


Now it's time to set the Domain Controller's NIC Private IP to **Static**

Go to the Domain Controller and click on the **Networking** tab.

After that, click on the *Network Interface*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e702cc27-b53c-42af-b002-3a7ca8322992" height="60%" width="60%" alt="9"/><br />
<br />

Now, go the **IP configurations** tab and click on the IP configuration. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/2d248eb4-13d7-42de-9945-1aa600fdae1d" height="60%" width="60%" alt="9"/><br />
<br />

Now, change the *Allocation* from **Dynamic** to **Static**.

Then click ***Save***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8d3b0db5-68cc-4626-a552-f62aac7574a7" height="60%" width="60%" alt="9"/><br />
<br />
<h2></h2>
<br />

After that, using the **User** and **Password** created before, login to the Client with its IP address in **Remote Desktop Connection**. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/44d9cfa3-7e43-4d67-a605-8f38414731dd" height="55%" width="55%" alt="9"/><br />
<br>

Then, using **Command Prompt**, ping the Domain Controller with its Private IP Address.

Type in "*ping (**Your DC Private IP**) -t*" to perpetually ping.<br>

For now it will time out.

```commandline
ping 10.0.0.4 -t
```


<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a9ac0069-9240-42ed-b61f-5606a900ec44" height="80%" width="80%" alt="9"/><br />
<br>

Following this, it's time to enable **ICMPv4**.<br>

First, login to the Domain Controller VM then open ***Windows Defender Firewall with Advanced Security***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d42ded65-aee3-4e87-bc61-a7b2b3050e50" height="85%" width="85%" alt="9"/><br />
<br>

Click on **Inbound Rules** and Sort by **Protocol**.

Look for the rules with ***Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)***.<br>

There will be two of them *(Both at the bottom of the image below)*

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/67f09539-fc53-451f-82dc-cc0cd9a4d77d" height="80%" width="80%" alt="9"/><br />
<br>

Right-click and **Enable** both rules.<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/83a47889-6fc4-40ec-aa6d-082ea4620238" height="55%" width="55%" alt="9"/><br />
<br>

Now go back to the **Client VM** and check on the *Command Prompt*.

✅ This time it should be properly pinging the Domain Controller.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/51bbc494-db1e-41c1-9f4c-6a84d55585b0" height="80%" width="80%" alt="9"/><br />
<br>
<br>

<h2>Step 2: Installing Active Directory</h2>

Now it's time to install **Active Directory**.

Go to the Domain Controller and in *Server Manager* click on *Add roles and features*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5c47459b-596d-4b61-a7ba-820a37bc50e8" height="80%" width="80%" alt="9"/><br />
<br>

Click *Next* until reaching the *Server Roles* section.

Check the box next to *Active Directory Domain Services* then *Add Features*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5a62a3a5-9570-4eef-af9f-36f97764d09f" height="80%" width="80%" alt="9"/><br />
<br>

Click *Next* until reaching the *Confirmation* tab then click *Install*.

It may take a while to install.<br>
<br>
Once it says "*Configuration required. Installation succeeded on (**Your DC name here**)*" 🡪 Click *Close*<br>
<br>
Towards the top-right corner of the *Server Manage*r window, there will be a flag and a yellow triangle with a ⚠️ symbol 🡪 Click on that.

Then click on "*Promote the server to a domain controller*".

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a067d615-f065-479a-97ef-65a23609406d" height="40%" width="40%" alt="9"/><br />
<br>

A window will pop up for a **Configuration Wizard**.

Check the bubble ◉ next to "*Add a new forest*"<br>
<br>
After that give it a domain name (example in the image below) and then click *Next*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e1bfed73-b689-4cbf-a950-56f6ee5341e9" height="80%" width="80%" alt="9"/><br />
<br>

Give it a DSRM password (required but won't be used in this tutorial).

Click *Next*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/04a4531f-502e-493e-a7cd-2264ccad85d8" height="80%" width="80%" alt="9"/><br />
<br>

Next, the **NetBIOS domain** will be made (this may take a while).<br>

Once it is made, click *Next* until reaching the **Prerequisites Check** tab, this process will take a moment.

Now click *Install*.

After Installing, the VM will rebooted.

Once it is rebooted 🡪 Log back into the Domain Controller with the *Domain Name* and the *Username*.<br>
<br>
Example below:

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/e597573e-b3fc-4d4e-b15a-5b7bd4a29f25" height="55%" width="55%" alt="9"/><br />
<br>
<br>

<h2>Step 3: Creating a Domain Admin</h2>

Once logged in: using Server Manager 🡪 click on **Tools** in the top-right corner.

Then click on ***Active Directory Users and Computers***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/6996f0fa-6cb3-4f8a-9238-6e5e36739787" height="70%" width="70%" alt="9"/><br />
<br>

In the *Domain Container* 🡪 Create a new **Organizational Unit**

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5360528b-d298-45f1-84df-4c2782193a4b" height="80%" width="80%" alt="9"/><br />
<br>

Name the Organizational Unit: ***_ADMINS***, then click ***OK***.

In the **_ADMINS** tab, create a *New* 🡪 *User*.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a66e8d6f-840f-4716-bc8c-aeda7c5f2650" height="80%" width="80%" alt="9"/><br />
<br>

Name this anything 🡪 Just remember the **User** and **Password**.

Uncheck the box ☐ that is next to "*User must change password at next logon*" 🡪 This won't be necessary.

Click ***Next*** then click ***Finish***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/40b6ddcb-7051-4e1a-82c3-2e757b060590" height="50%" width="50%" alt="9"/><br />

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/23ffe8b0-95db-48ac-ba08-9f549e7a95b0" height="50%" width="50%" alt="9"/><br />
<br>

Now add this User to the ***Domain Admins** security group*.

Right-click on the User created, then click ***Properties***.

Click on the ***Members of*** tab, then click ***Add***. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/1186dab4-fb57-4377-b3e9-273f4e196417" height="50%" width="50%" alt="9"/><br />
<br>

Type ***domain*** in the box under "*Enter the object names to select*" 🡪 then click ***Check Names***. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/7afd3274-1173-43a3-ae2c-0768dfcb3c27" height="60%" width="60%" alt="9"/><br />
<br>

Choose the ***Domain Admins*** option then click ***OK***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/07778ba0-be50-4493-b5ad-f88f6125ae62" height="80%" width="80%" alt="9"/><br />
<br>

Now, click ***Apply***.

✅ The User has successfully been added to the Domain Admins security group 🡪 click ***OK***.

Now logout of the **Domain Controller** and re-login as the User just created.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/6a366cad-ec53-4439-acf3-5fd556203c0a" height="50%" width="50%" alt="9"/><br />
<br>



<h2>Step 4: Setting Client DNS Settings to Domain Controller Private IP Address</h2>
<br>

First, in *Azure*, go to the **Client VM**.

Next, go to the **Networking** tab and click on the ***Network Interface***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/f721f2cd-57b4-4859-8d4f-ef7458fc836c" height="60%" width="60%" alt="9"/><br />
<br>

Next, go to the **DNS Servers** tab and create a ***custom DNS Server***.

Add a custom server using the *Domain Controller's Private IP address*.

Example Below:

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/0bffe71f-6738-4262-8fbc-b7cfe1bcb396" height="60%" width="60%" alt="9"/><br />
<br>

Now click ***Save***.

Next go back to the **Client VM** and click ***Restart*** in the *Overview tab*. 

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a8b568af-86cc-483e-a80c-0bab61f249e2" height="80%" width="80%" alt="9"/><br />
<br>

Once the Client is restarted 🡪 Login to the Client with ***Remote Desktop*** as the **Admin Account** created.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/7dc19edc-c69c-4b8b-9c84-4afea551f703" height="50%" width="50%" alt="9"/><br />
<br>

After login in:<br>
Go to **Settings 🡪 System 🡪 About** and click on ***Rename this PC (advanced)***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/19aec300-b816-4a43-81d9-0b26828c90ab" height="80%" width="80%" alt="9"/><br />
<br>

Now Click on "***Change...***"

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/425dde87-0b5a-45f5-87f2-9eae099c65bd" height="50%" width="50%" alt="9"/><br />
<br>

Now check the bubble ⦿ next to ***Domain*** then type in the *Domain Name* (**Your own domain name**).

There should be a window that pops up for a login 🡪 Use the admin previously created to login.

Example below:

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/697f46b6-b338-4e0a-a037-f14521d63f45" height="80%" width="80%" alt="9"/><br 
                                                                                                                                                               
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/a2dcdf55-a180-4731-9088-384dd8eb3cfb" height="40%" width="40%" alt="9"/><br />

✅ Success.

The VM will now restart after a short period.
<br>
<br>
<br>


<h2>Step 5: Setting up Remote Connection for Domain Users</h2>

Now, log into the Domain Controller.

Go back to **Server Manager 🡪 Tools 🡪 Active Directory Users and Computers**.

Under the Domain container, go to the **Computers** tab: it should show that the client has been added to the list.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/78e35fd9-c8b1-4ff2-9d92-d259697b033b" height="80%" width="80%" alt="9"/><br />
<br>

Now, log into the **Client** as the *Admin User* created and go to **System Settings 🡪 Remote Desktop**.

Click on ***Select users that can remotely access this PC***.

Next click ***Add***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/72f37e50-337d-4f5c-a849-c5d0745c3097" height="80%" width="80%" alt="9"/><br />
<br>

In the box at the bottom: type in "***Domain Users***" and click on ***Check Names***.

Next click ***OK***.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/38b39f41-9ebf-4df2-a887-ae0dcc81ee6a" height="60%" width="60%" alt="9"/><br />
<br>
<br>
<br>

<h2>Step 6: Creating Domain Users</h2>

In the Domain Controller, open "***Windows PowerShell ISE***".

Make sure to open it as *Administrator*.



<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/db4a5dde-f726-4ad9-9a5c-1c6db7097a4e" height="85%" width="85%" alt="9"/><br />
<br>
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/8dffa873-a847-44ad-97a3-111b41ce01eb" height="85%" width="85%" alt="9"/><br />
<br>

Next, download de ***script*** from the following link:

https://github.com/franciscovfonseca/Active-Directory-Powershell/blob/master/1_CREATE_USERS.ps1
<br>
<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/f6a2db1f-23e6-4456-bb3f-25be58081e64" height="85%" width="85%" alt="9"/><br />
<br>
<br>

After downloading the file, you want to open the script trough **Powershell** as shown in the images bellow:

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/9f3b3267-25f0-4b49-9b56-752fb8099a3f" height="85%" width="85%" alt="9"/><br />

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/0a5ab8ee-93ca-44a7-8c6d-e9d695ed1993" height="80%" width="80%" alt="9"/><br />
<br>

Run "***Set-ExecutionPolicy Unrestricted***" in the command line.

```commandline
Set-ExecutionPolicy Unrestricted
```
<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/5a6727cf-0dd6-40ff-871d-9157415d059a" height="65%" width="65%" alt="9"/><br />
<br>

Before running the script, in order for us to be able to pull in the ***\names.txt***, we need to go to the actual directory where the script is at, and change it to make it work.

So still in the command line 🡪 we type in the following script:

```commandline
cd C:\Users\tsmith\Desktop\1_CREATE_USERS.psy
```
<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/b0a49b5a-20b7-42dd-9a67-092faec71461" height="65%" width="65%" alt="9"/><br />

(*In this case above we typed in **tsmith**, but it should be whatever user account we logged into.*)

<br>



Now click the **Run** button to *run the script*.

This will start creating *Domain Users* with **Usernames** and **Passwords** (The Password for these users will be "Password1").

Example below:

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/d3c053ba-6310-4766-a6dc-381404c083f9" height="65%" width="65%" alt="9"/><br />
<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/668e20b8-faa8-456a-b3d2-9d37f2486590" height="80%" width="80%" alt="9"/><br />
<br>

Go to Server **Manager 🡪 Tools 🡪 Active Directory Users and Computers**.

Under the ***_USERS*** tab, look at all of the users created from the script.

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/f0c344b1-d1c2-4683-a20b-91bcb41e8704" height="80%" width="80%" alt="9"/><br />

These names are all randomly generated.


Choose one and log into the **Client VM** with the username it is assigned: (Remember the password is ***Password1***)
<br>
<br>

<img src="https://github.com/franciscovfonseca/Active-Directory-Lab/assets/172988970/7c836b5e-e5d7-4239-a388-c512e5175446" height="50%" width="50%" alt="9"/><br />
<br>

✅ **Congrats! You completed this tutorial**.
<br>
<br>
<br>

<h2>Conclusion</h2>

**Active Directory** is crucial for organizations to effectively *Control their Network Traffic* and *Prevent Unauthorized Access to Internal Networks* or *Leakage of Information to External Parties*.

Understanding and learning about **Active Directory** is a fundamental principle for all IT professionals, regardless of their specific roles.

Hope you found this tutorial both informative and valuable!
<br>
<br>
<br>




