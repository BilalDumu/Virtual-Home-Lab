<h1 = align=center>Home lab - Active Directory</h1>
<h2 = align=center>Adding Users with PowerShell</h2>

<p align="center">
<img width="1342" height="883" src="https://i.imgur.com/ewpdeDP.png" />
</p>

---

## 🛠️ TECHNOLOGIES & TOOLS UTILIZED

- **`VirtualBox:`**  
  Used as the primary virtualization platform to host multiple virtual machines in an isolated local environment for cybersecurity testing and infrastructure simulation.

- **`Windows Server 2019:`**  
  Deployed as the domain controller within the lab. Configured to run Active Directory Domain Services (AD DS), DNS, and Group Policy to emulate enterprise-level identity and access management scenarios.

- **`Windows 10:`**  
  Installed as a domain-joined workstation to simulate a typical enterprise endpoint. Used for user behavior simulation, GPO testing, and endpoint visibility through logging tools.

- **`Active Directory:`**  
  Implemented to manage user accounts, groups, and organizational units (OUs). Used for hands-on experience with authentication flows, access control, privilege escalation testing, and administrative scripting.

- **`PowerShell:`**  
  Used for automating administrative tasks such as creating users, managing AD objects, and configuring system settings within both the server and client machines.
  
- **`Group Policy Management:`**  
  Applied GPOs to enforce password policies, audit policies, restrict user privileges, and configure security baselines across the domain environment.

---

## OBJECTIVE

Designed and deployed a virtualized Windows domain lab using `VirtualBox`, `Windows Server 2019`, and `Windows 10` to gain hands-on experience with enterprise network infrastructure and identity management. The project focused on configuring `Active Directory`, `Group Policy`, and domain-joined `endpoints` to simulate real-world authentication, access control, and administrative workflows. This lab environment served as a foundation for practicing common security tasks, including privilege escalation prevention, user access auditing, and scripting automation with `PowerShell`.

## 👣 STEP-BY-STEP: SETTING UP THE DOMAIN CONTROLLER

### Step 1: `Server 19` Virtual Machine Creation and Provisioning

The virtual machine, named `Domain-Controller`, was configured as a domain controller using the `Windows Server 2019 ISO`. It was provisioned with 2048 MB of RAM and 4 virtual CPUs to support `Active Directory` and core infrastructure services.
- For networking, `Network Adapter 1` was enabled and attached to `NAT` to allow internet access. `Network Adapter 2` was connected to an `internal network` to support isolated communication between lab machines without external exposure.
 
<img width="1342" height="883" src="https://i.imgur.com/f1AE44b.png" /></br>

### Step 2: `Windows Setup`

Powered on the virtual machine and proceeded through the Windows Server 2019 installation process using the ISO image.

<img width="636" height="476" src="https://i.imgur.com/rp2PKck.png" /></br>

Windows Server 2019 Standard Evaluation (Desktop Experience) → Accept the license terms → Username: Administrator → Password: Password|1

---

### Step 3: Check `Network Configuration`

Checked the `network connection details` of the virtual machine and observed it was automatically assigned an IPv4 address of `169.254.185.53`, indicating an `APIPA` (Automatic Private IP Addressing) address.

<img width="357" height="434" src="https://i.imgur.com/WnUYoHx.png" /></br>

Under `Network Connections`, we renamed the NAT adapter to `Internet` and the internal network adapter to `Internal_Network` for better clarity. The `Internet_Network` adapter was then manually configured with the following static IP settings:  
- **IP address:** `172.16.0.1`  
- **Subnet mask:** `255.255.255.0`  
- **Preferred DNS server:** `127.0.0.1`

<img width="782" height="159" src="https://i.imgur.com/z2HwnaY.png" /></br>

<img width="755" height="455" src="https://i.imgur.com/0Eyxtyp.png" /></br>

---

### Step 4: Set Up `Active Directory`

In `Server Manager`, we began by launching the `Add Roles and Features` to install `Active Directory Domain Services` (AD DS).

<img width="755" height="455" src="https://i.imgur.com/Z8jDlwo.png"/></br>
<img width="755" height="455" src="https://i.imgur.com/Q1HU6UM.png"/></br>

During the setup, we also included `Group Policy Management` and `Remote Server Administration Tools` (RSAT) to enable centralized domain control and administrative functionality.

<img width="755" height="455" src="https://i.imgur.com/t5C1nm4.png"/></br>

<img width="755" height="455" src="https://i.imgur.com/eRbGgNw.png " /></br>

After completing the role installation, we proceeded with the `post-deployment configuration` by promoting the server to a domain controller.

<img width="755" height="455" src="https://i.imgur.com/AM08nbA.png " /></br>

During the configuration, we created a new forest with the root domain name `mydomain.com`.

<img width="755" height="455" src="https://i.imgur.com/WD03RKf.png " /></br>


Enabled both the `Domain Name System` (DNS) server and `Global Catalog` (GC) options. 

<img width="755" height="455" src="https://i.imgur.com/fUF0Sbe.png " /></br>

We also set the NetBIOS domain name to `MYDOMAIN`.

<img width="755" height="455" src="https://i.imgur.com/ezYA1Zj.png " /></br>

---

### Step 5: Set up `Domain Admins` and `Domain Users`

After logging back into the server, we navigated to `Active Directory Users and Computers` to begin configuring domain objects.

<img width="755" height="455" src="https://i.imgur.com/TUIDmlV.png" /></br>


We created a new `Organizational Unit` (OU) named `_ADMINS`, and within that OU, added a new user account with the full name `Bilal Dumu` and the user logon name `a-bdumu`.

<img width="433" height="374" src="https://i.imgur.com/qv7nbEe.png " /></br>

Within `Bilal Dumu’` account properties in `Active Directory Users and Computers`, we navigated to the `Member Of` tab and added the user to the `Domain Admins` group. This granted him administrative privileges across the domain. 

<img width="755" height="455" src="https://i.imgur.com/IWinUZc.png" /></br>

---

### Step 6: Set up `Remote Access` Services

In `Server Manager`, we returned to `Add Roles and Features` to install the `Remote Access role`. During the setup, we selected the role services `DirectAccess and VPN` (RAS) as well as `Routing` to enable secure remote connectivity and traffic management for the network.

<img width="781" height="555" src="https://i.imgur.com/YsGMNC9.png " /></br>

Next, we navigated to `Tools` > `Routing and Remote Access` and launched the Routing and Remote Access Server Setup Wizard.

 <img width="616" height="437" src="https://i.imgur.com/1NsA7J2.png " /></br>

We proceeded to configure `NAT` by selecting our internet-facing network interface, named `Internet`, to provide network address translation for internal clients.

 <img width="616" height="437" src="https://i.imgur.com/oqt43Cv.png " /></br>
 <img width="616" height="437" src="https://i.imgur.com/L4mqmdG.png " /></br>
 
Upon successful configuration, the domain controller (local server) displayed a green status arrow, indicating the service was running properly.

 <img width="616" height="437" src="https://i.imgur.com/FiGj2cr.png " /></br>

---

### Step 8: Set up `DHCP Server`

Back in `Server Manager`, we launched the `Add Roles and Features` Wizard again to install the `DHCP Server` role, enabling the server to assign IP addresses dynamically within the network.

<img width="779" height="554" src="https://i.imgur.com/2JwWV0H.png   " /></br>

We navigated to `Tools` > `DHCP` and launched the `New Scope Wizard` to configure a DHCP scope.

<img width="779" height="554" src="https://i.imgur.com/tC41SKH.png " /></br>

We named the scope `172.16.0.100-200` and set the IP address range from `172.16.0.100` to `172.16.0.200` with a subnet mask of `255.255.255.0` (prefix length 24). The router (default gateway) was set to `172.16.0.1`, and the parent domain was specified as `mydomain.com`.

<img width="779" height="554" src="https://i.imgur.com/fw0F4Vy.png  " /></br>

After activating, authorizing, and refreshing the DHCP server, its status displayed a green icon indicating it was functioning properly.

<img width="779" height="554" src="https://i.imgur.com/DzaTxwp.png " /></br>

---

