<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial touches on implementing Active Directory within the Azure Virtual Machines in Azure Portal <br />
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
First we will create two virtual machines both using the same Vnet and resource group. The first VM we will create will serve as our domain controller and we will name it "DC-1" and it will be (Windows Server 2022) We will also create another VM called client-1. We will also set the Domain Controller NIC private IP address to be static. Client-1 will essentially be used to join DC-1. Client-1's DNS servers will be the same as the domain controllers. 
</p>
<br />

<p>
<img src=<"https://i.imgur.com/HvZBWzc.png" height="80%" width="80%"/>
</p>
<p>
Log into client-1 with remote desktop and attempt to ping DC-1's private IP address. You will notice the ping will not be successful. We will have to log into DC-1 using remote desktop and ICMPv4 in on the windows firewall. After we do that we log back into client-1 and the ping will succeed. 
</p>
<br />

<p>
<img src="https://i.imgur.com/09rtEUq.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/8UQUZOT.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Log back into DC-1 and from there we will install Active Directory Domain Services. We now promote the VM to a DC. Then we setup as a new forest named "mydomain.com" or anything you'd prefer just be sure to remember what you put down as the name. After installation it will force us to log off and restart. Proceed to log back into DC-1 as "mydomain.com\labuser" 
</p>
<img src="https://i.imgur.com/X04dlRP.png" height="80%" width="80%"/>
<img src="https://i.imgur.com/shBBQsm.png" height="80%" width="80%""/>
<br />
</p>
Within Active Directory Users and Computers, we shall now create an organizational unit called "_EMPLOYEES" Afer that let's go ahead and create another organizational unit and we'll name it "_ADMINS" Next we will create a new employee called "Jane Doe" with the user name of "Jane_Admin" Then we will add Jane Doe to the Domain Admins Security Group. 
</p>
<img src="https://i.imgur.com/i3qys6Y.png" height="80%" width="80%"/>
<br />
</p>
<img src="https://i.imgur.com/lfQKCtS.png" height="50%" width="50%"/>
Now we can join Client-1 to the domain. Let's go back to the Azure Portal and set Client-1 DNS settings to domain controller's private IP address. After we do this go ahead and restart Client-1 in the Azure Portal. Next let's log back into Client-1 as the orignial local admin and join it to the domain. Afterwards log back into DC-1 and make sure Client-1 shows up in Active Directory Users and Computers inside the container called "computers." 
</p>
<img src="https://i.imgur.com/fCKPC0j.png" height="80%" width="80%"/>
<br />
</p>
<img src="https://i.imgur.com/C6KbmJo.png" height="80%" width="80%"/>
</p>
<p>
</p>
<p>
We will continue by setting up remote desktop for non-administrative users on Client-1. Log into Client-1 as the admin and open system properties. Click on "Remote Desktop" and allow domain users access to the remote desktop. Now we are allowed to log into Client-1 as a non-administrative user now!
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/81TINJg.png" height="80%"/>
</p>
<p>
Let's log back into DC-1 in our admin account. Open up PowerShell_ise as an administrator. We will create a new file on PowerShell_ise and paste the contents of a script that will generate ten thousand users in the domaian. We will run the scripts and observe the accounts created at a rapid pace.  Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/kKSqkwo.jpg" height="80%" width="80%"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/14WX5Om.jpg" height="60%" width="60%"/>
</p>
<img src="https://i.imgur.com/ptXfDSW.jpg" height="60%" width="60%"/>
<p>We have logged into Client-1 as one of the ten thousand users created "bafi.dox" by PowerShell_ise and the scripts contents we pasted onto their. We logged into this account with this users credentials as a normal user.  </p>

