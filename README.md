<p align="center">
<img src="https://i.imgur.com/Ucqw15T.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>🧠 How to Install Active Directory & Join a Client to the Domain in Azure (Part 2)</h1>
<p>🚀 A continuation of our hands-on lab for setting up a fully functioning Active Directory environment using Azure Virtual Machines.</p>

<h1>🧠 Overview</h1>
<p>✅ This post covers installing Active Directory Domain Services (ADDS), promoting the Domain Controller, and joining a Windows 10 client to the domain.</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

## 🔧 PART 1: Installing and Configuring Active Directory Domain Services (AD DS)

1. Log into the DC-1 VM
   - Use Remote Desktop (RDP)
   - Enter your credentials:
	  - Username: `labuser`
      - Password: `Cyberlab123!`
 
2.	Install Active Directory Role
   - Open `Server Manager`
   - Click `Add Roles and Features`
   - Click through the first few screens
   - Under `Server Roles`, check:
	  - `Active Directory Domain Services`
      - Then click `Add Features` when prompted
   - Click `Next` > `Install`
  
3. Promote DC-1 to a Domain Controller
  - After the role installs, click Promote this server to a domain controller
  - In the wizard:
	• Select `Add a new forest`
	• Root domain name: `mydomain.com` (or anything you choose)
	• Set a DSRM password: `Cyberlab123!`
  - Click through the rest of the steps and hit `Install`
  - The VM will automatically reboot
   
4.	Create an Admin User (jane_admin)
   - After reboot, log into `DC-1` with:
	  - Username: `mydomain.com\labuser`
      - Password: `Cyberlab123!`

5.	Create an Admin User (jane_admin)
   - Open `Server Manager`
   - Go to `Tools` > `Active Directory Users and Computers`
   - Expand `mydomain.com`
   - Right-click your `domain` > `New` > `Organizational Unit`
       - Create two OUs:
	      •	`_EMPLOYEES`
	      • `_ADMINS`
   - In `_ADMINS:`
     - Right-click > `New` > `User`
     - Fill in:
	    - First Name: `Jane`
        - Last Name: `Doe`
        - Username: `jane_admin`
        - Password: `Cyberlab123!`
        - Uncheck `“User must change password at next logon”`
    - After creating Jane’s account:
	  -  Right-click `Jane` > `Add to a group`
        - Enter: `Domain Admins` > `Check Names` > `OK`

***🟢 You now have a domain admin account!***

6. Switch to Jane’s Admin Account

- Log out of `DC-1`
  - Log back in with:
	- Username: `mydomain.com\jane_admin`
    - Password: `Cyberlab123!`

***Use this account going forward.***

-----

## 🖥️ PART 2: Join the Client-1 VM to the Domain

1. Verify DNS and Restart
  - Ensure `Client-1` is using `DC-1`’s private IP as its DNS
  - Restart the VM from the Azure Portal if changes were made

2. Log into `Client-1` via RDP
  - Use:
	- Username: `labuser`
	- Password: `Cyberlab123!`

***This allows normal users (not admins) to log in remotely.***

3. Join to Domain
  - Open `System Properties`
  - Under Computer Name, click `Change settings`
  - Click `Change…` and choose `Domain`
  - Enter: `mydomain.com`
  - When prompted:
   - Username: `mydomain.com\jane_admin`
   - Password: `Cyberlab123!`
  - Click `OK`, then `Restart` when prompted

4. Verify Client in Active Directory
  - RDP into `DC-1` as `jane_admin`
  - Open `Active Directory Users and Computers`
  - You’ll see `Client-1` in the Computers folder
  - Right-click `Client-1` > `Move`
   - Create and move it to a new OU: `_CLIENTS`

***✅ Your domain is working! The client is joined!***

-----

### 🧪 PART 3: Domain Logins and Remote Access

1. Ensure Both VMs Are On
  - Start both `DC-1` and `Client-1` in the Azure Portal

2. Enable RDP for Domain Users on `Client-1`
  - Log into `Client-1` as `mydomain.com\jane_admin`
  - Go to:
	- `System Properties` > `Remote Desktop`
    - Click `Select Users…`
    - Add: `Domain Users`
    - Click `Check Names` > `OK`

***💡 Now standard domain users can log in via RDP.***

3. Bulk Create Test Users
  - RDP into `DC-1` as `jane_admin`
  - Open `PowerShell ISE` as Admin
  - Paste this [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
  - Run it to generate a list of user accounts

4. Test Logins from `Client-1`
  - RDP into `Client-1`
  - Log in with a test user:
	- Username: `mydomain.com\testuser1`
    - Password: `Cyberlab123!`

***✅ If successful, you’ve confirmed domain login and remote access work!***

-----

## ✅ Conclusion

You’ve completed the second part of your Active Directory lab, where you:
	
 - Installed and configured Active Directory Domain Services
 - Promoted DC-1 to a domain controller
 - Created an admin user (Jane Doe) and joined Client-1 to the domain
 - Verified communication and enabled Remote Desktop access for domain users

🔜 Next Step to third: [Creating Users using Powershell](https://github.com/anumkhanit/create-users-powershell)
