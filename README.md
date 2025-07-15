<p align="center">
<img src="https://i.imgur.com/Ucqw15T.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>How to Install Active Directory & Join Client to Domain in Azure</h1>
<p>Hands-On Lab: Part 1 & Part 2</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

## PART 1: Installing & Configuring Active Directory Domain Services

1. **Install Active Directory on DC-1**
	 - Log into `DC-1` via Remote Desktop (RDP) using:
	  - Username: (remember the easy username you made? Use that)
	  - Password: (remember the easy password you made? Use that)
 
2.	**Open Server Manager**
    - Click `Add Roles and Features`
    - Click `Next` through the first few screens
    - Under `Server Roles`, check `Active Directory Domain Services`
    - Click `Add Features` when prompted
    - Click `Next` and then `Install`
  
3.	**After it installs, click `Promote` this server to a domain controller**
   
4.	**In the wizard:**
    - Select `Add a new forest`
    - Root domain name: `mydomain.com` (or anything you like)
    - Click `Next`, set a DSRM password (you can use Cyberlab123!)
    - Keep clicking `Next`, then `Install`

5.	**Your server will restart**

6.  **Log Back into `DC-1` as a Domain User**
	  - After reboot, RDP back into `DC-1`
    - Log in with your domain account:
	    - Username: `mydomain.com\labuser`
      - Password: `Cyberlab123!`

7.  **Create a Domain Admin User (Jane Doe)**
     - Open Server Manager
     - Go to `Tools` > `Active Directory Users and Computers (ADUC)`
     - In the left pane, expand your domain `mydomain.com`
     - Right-click the `domain` > `New` > `Organizational Unit (OU)`
       - Name it: `_EMPLOYEES`
     - Repeat and create another OU: `_ADMINS`

8.	**In `_ADMINS`, right-click > `New` > `User`**
     - First Name: Jane
     - Last Name: Doe
	- Username: `jane_admin`
  - Password: `Cyberlab123!`
	- Uncheck `User must change password…`

9. **After creating Jane’s account:**
   - Right-click `Jane` > Add to a group
	 - Type `Domain Admins` > Click `Check Names` > `OK`

10.	**Log out of `DC-1`, then log back in as:**
	  - Username: `mydomain.com\jane_admin`
	  - Password: `Cyberlab123!`

***Use this account from now on***

11. **Join `Client-1` to the Domain**
	  - From the Azure Portal, verify:
	   - `Client-1`’s DNS settings are set to `DC-1`’s private IP
	  - VM has been restarted

12.	**RDP into `Client-1` using:**
	 - Username: `labuser`
	 - Password: `Cyberlab123!`

13. **On `Client-1`:**
	 - Go to `System Properties`
	 - Under Computer Name, click `Change settings`
	 - Click `Change…` button
	 - Select Domain, and enter: `mydomain.com`
	 - When prompted, use:
	  - Username: `mydomain.com\jane_admin`
	  - Password: `Cyberlab123!`

14. **Click `OK` and restart the computer when prompted.**

15. **Verify in ADUC**
    - RDP back into DC-1 as `jane_admin`
    - Open `Active Directory Users and Computers`
    - You should now see `Client-1` listed in the default `Computers` folder
    - Right-click `Client-1` > `Move`
      - Move it into the OU: `_CLIENTS` (create this OU if not already made)

***You’ve now successfully joined a client to the domain!***

-----

## PART 2: Domain Logins and Remote Access

1. **Turn On VMs If They’re Off**
   - In the Azure Portal, start `DC-1` and `Client-1`
   - Wait for both to fully boot before connecting

2. **Allow Remote Desktop Access for Domain Users `Client-1`**
	 - RDP into `Client-1` using `mydomain.com\jane_admin`
	 - Go to:
	  - `System Properties` > `Remote Desktop`
	 - Click `Select Users…`
	 - Add: `Domain Users`
	 - Click `Check Names` > `OK`

***This allows normal users (not admins) to log in remotely.***

3. **Create Multiple Test Users in PowerShell**
	 - RDP into `DC-1` as `jane_admin`
	 - Open `PowerShell ISE` as Administrator
	 - Paste the following [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into a new file

4. **Test User Login on `Client-1`**
	 - From the Azure Portal, RDP into `Client-1`
	 - When prompted to log in, use one of the [scripts](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) to make new accounts:
	  - Username: `mydomain.com\testuser1`
	  - Password: `Cyberlab123!`

5. **If login succeeds, you’ve confirmed that domain user accounts are working and Remote Desktop is enabled for them!**

-----

## Conclusion

You’ve successfully installed Active Directory, promoted a Domain Controller, created admin and test user accounts, joined a client to the domain, enabled Remote Desktop for non-admins and tested domain logins with new accounts.

Move onto the next part: [Creating Users using Powershell](https://github.com/anumkhanit/create-users-powershell)
