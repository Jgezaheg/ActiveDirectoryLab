<h1>Active Directory Homelab</h1>

<h2>Description</h2>
In this lab we're going to walk through how to create an Active Directory home lab Environment using Oracle Virtual box. Configuring and running this lab will help develop my understanding of how active directory and windows networking works.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b>
- <b>Oracle Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Server 2019</b>


<h2>Program walk-through:</h2>

<h3><p align="center">
1. Install Virtual box and Create a VM named DC (Domain Controller):</h3> <br/>
 
![CreateVM-DomainController](https://user-images.githubusercontent.com/129562058/229259180-4d677f7c-b152-4740-9a2f-21fc810c9db5.png)<br/>
 
 <h3><p align="center">
 2. Assign 2 GBs of RAM and 4 cpu cores to the VM:</h3> <br/>
 
![Setting up memory and processing for VMs](https://user-images.githubusercontent.com/129562058/229258595-4db76e04-6d4a-4c35-9190-4965d01c46ae.png)<br/>

<h3><p align="center">
 3. Set up 2 NIC adapters:</h3> <br/>
 
 ** Adapter 1: dedicated to the internet = NAT**<br/>
 ** Adapter 2: dedicated to internal VM ware Network = Intnet**
 
 ![adding NICs to vm one-NAT-internet](https://user-images.githubusercontent.com/129562058/229260101-8673cd67-2cb7-4030-8d03-fbc370562333.png)
 ![adding NICs to vm two-internal-intnet](https://user-images.githubusercontent.com/129562058/229260057-e02442fe-38d0-4195-97ca-cb5f30703d0e.png)<br/>
 
<h3><p align="center">
4. Open the DC vm, select and open the Server 2019 ISO file :</h3> <br/>

![Sever2019ISO_to_DC](https://user-images.githubusercontent.com/129562058/229259459-cde0330d-38ca-4cc7-b267-c1b2452610f9.png)<br/>

<h3><p align="center">
5. Setup Windows Sever 2019 follow instructions presented on the screen :</h3> <br/>
**select either desktop experience OS during set up for GUI**
**Dont press any keys during reboot process**

![WinServer2019 Setup](https://user-images.githubusercontent.com/129562058/229259560-d55eb2a4-b47c-4d77-86b2-b8f9f7f12291.png)<br/>

<h3><p align="center">
6. Setup IP Addressing :</h3> <br/>
**NIC (Internet): Automatically gets IP from home Router**<br/>
**NIC (Intnet or Internal): Needs manual setup**<br/><br/>
**Select network icon located in the bottom right -> select network -> select change adapter options -> identify which network is which**<br/><br/>
**Interet will automatically have the ip address assigned to your home network** **internal will need to have an ip address assigned to it**

![setting up ip addressing](https://user-images.githubusercontent.com/129562058/229261156-61fdbbb1-f9aa-494f-a588-a2f9fd5bf6e1.png)<br/><br/>

setup ip address for the internal network<br/><br/>
**right click the internal network -> select proerties -> double click ipv4 -> select use the following ip addresses**<br/><br/>
insert the following when prompted:<br/>
IP address: 172.16.0.1<br/>
Subnet mask: 255.255.255.0<br/>
Default gateway: **no default gateway because the DC serves as one**<br/><br/>

Prefered DNS: 127.0.0.1 ** computer uses itself as DNS by utilizing loop back address (127.0.0.1)**<br/>

![internalIPsetup](https://user-images.githubusercontent.com/129562058/229261693-d56594c3-f10e-47cb-89f3-e979e423b4b0.png)<br/>

<h3><p align="center">
7. Install Active Directory Domain Services (AD DS) :</h3> <br/>
Navigate to the Server Manager:<br/>
**Select add roles and features -> select next -> select next -> select the server named DC (only one should be available) -> under server roles select "active directory domain services" -> add roll -> select next until prompted to install.<br/>

![installingADDS](https://user-images.githubusercontent.com/129562058/229262262-02f7dc7c-450c-46a0-a0fb-735f5517588d.png)<br/>

Next select the flag in the top right of the Sever Manager to begin **post-deployment-configuration** -> select promote this sever to a domain controller -> select add new forest and name the Root domain anything you want -> select next -> add a password when prompted -> select next until prompted to install -> **VM will restart**.<br/><br/>
After restarting your sign in screen will have changed to include what ever you named the root domain in the last step (in my case it was mydomain.com)<br/>

![loginAfterADDS set up](https://user-images.githubusercontent.com/129562058/229263060-8c59e459-da08-4397-a253-d226296a92b6.png)<br/><br/>

<h3><p align="center">
8. Create dedicated admin account instead of using build in admin account (AD DS) :</h3> <br/><br/>
**Select the start menu -> Windows Administrative Tools -> Active directory users and computers -> Select the newly created domain** **(in my case mydomain.com)**<br/><br/>
Create a orginizational unit to put admin account in: **Right click your created domain -> select new -> select oranizational domain - Name it accordingly (in my case _ADMINS_)<br/>

![mydomainadminacc](https://user-images.githubusercontent.com/129562058/229263492-5de61f66-4ae1-416e-ac98-3317353b4213.png)<br/><br/>

inside of the new organizational domain create a new user-> enter name, username and password when prompted -> check password never expires for the sake of the lab enviornment -> select finish<br/>

Right click the created user -> properties -> memeber of -> add -> enter "Domain Admins in text box -> Check Names -> click ok and apply.<br/>

![creatingDomainAdminAccount](https://user-images.githubusercontent.com/129562058/229263931-0c36f3f7-0c69-4664-8ad8-0f19d45463cd.png)<br/><br/>

log out and sign in with other to sign in to the newly created domain (in my case MYDOMAIN)<br/>

<h3><p align="center">
9. Install RAS (Remote Access Server) and NAT (Network address translation) (AD DS) :</h3> <br/><br/>
Purpose: when Windows 10 client is created it will allow the client to be on a sort of private virtual network but sill be able to access the internet through the domain controller.<br/><br/>

On the Server Manager navigate to add roles and features -> select next -> select next -> select the server named DC.mydomain.com (only one should be available) -> under server roles select "Remote Access" -> add roll -> select next until "Role Services" -> select rounting -> add feature (direct access and features will be automatically selected along side routing) -> Select next until prompted to install.<br/><br/>
<br/><br/>
After the install finishes navigate to tools in the top right -> Routing and Remote Access<br/>

![configRemoteAccess](https://user-images.githubusercontent.com/129562058/229264926-26d26dcc-625d-4e1c-b0a0-025583d6c19e.png)<br/><br/>

right click the DC -> configure and enable Routing and Remote Access -> Next -> Select the NAT option -> Make sure the option "Use this public interface to connect to the internet" is selected and shows the previously set up NIC Networks that were created in the previous steps -> Select the one named _Internet_ -> Select finish <br/><br/>

![setting up NAT](https://user-images.githubusercontent.com/129562058/229265167-73685ea4-8867-4813-8086-c81cef13a102.png)<br/><br/><br/><br/>
Make sure the DC has the green indicator rather than red see image below<br/><br/>

![green routing and remote access](https://user-images.githubusercontent.com/129562058/229265255-01c5d066-aecf-4069-9c8e-ee6c116b484d.png)<br/><br/><br/><br/>

<h3><p align="center">
9. Set up DHCP Server on Domain Controller:</h3> <br/><br/>
This will allow the windows 10 clients to get an ip adress that will allow them to access the internet even though they are on a private/internal network similarly to an office or school enviornment <br/>

Go to the Server manager -> add roles -> select server (there should only be one similar to the previous steps) -> server roles -> select DHCP Server -> add features -> select next unitl install <br/><br/>

![DHCP](https://user-images.githubusercontent.com/129562058/229265775-9f66f409-7382-4efb-8c20-5d11be7bdea5.png)<br/><br/>

Next set up the scope: select tools in the top right -> DHCP <br/>
Notice how both ips under the domain are down with a red indicator<br/><br/>

![ip down](https://user-images.githubusercontent.com/129562058/229265906-99c3f135-9298-45f4-ba28-b7f57d5a7f4e.png)<br/><br/>

To fix: Right click one in my case ipv4 -> new scope -> name the scope after whatever the ip range is ( 172.16.0.100-200) -> set ip range<br/>

![set ip range](https://user-images.githubusercontent.com/129562058/229266109-2d247a95-2530-4adf-b389-57641d1df4b1.png)<br/><br/>

skip exclusions and set lease durations to 8 days for the sake of the lab -> select yes to DHCP options -> set default gateway (use domain controllers ip address with NAT configured -> Select next until prompted to finish<br/>
if indicators are still red right click the DHCP server and select authorize<br/><br/>

![Scope](https://user-images.githubusercontent.com/129562058/229266314-a2ae3946-97f7-4c3d-b5dc-bfaa8d995bbf.png)<br/><br/>

<h3><p align="center">
10. Creating 1000 user accounts using PowerShell</h3> <br/><br/>

Firstly open internet explorer in the domain controller -> paste this link to Download the script used in PowerShell as well as the list of users first and last names (randomly generated) -> open the folder and go to the names file -> add your name to the list to create an account for yourself<br/>

Next, got to the start menu -> select Windows PowerShell -> right click Windows powershell ISE -> select run as administrator -> open PowerShell CREATE_USERS script previously downloaded<br/>

For the sake of the lab and to be able to run the PowerShell script: In PowerShell type the command: Set-ExecutionPolicy Unrestricted -> Run Script<br/> make sure file is in the right directory by typing the file location into PowerShell: ex cd C:\users\a-jgezahegne\Desktop\AD_PS-master
<br/><br/>

![psscript](https://user-images.githubusercontent.com/129562058/229271289-a1be35d2-1adf-4416-8819-1d6a7b7be117.png)<br/><br/>

<h3><p align="center">
10. Create a Windows 10 VM in Virtual box</h3> <br/><br/>

Create the virtual machine the same way demonstrated in steps 1 and 2 but this time select the OS to be Windows 10 (64-bit)<br/>
Also when setting up the NIC adapter set up only one and select Internal Network for adapter 1 (to get DHCP address from domain controller to emulate office or school enviornment)<br/><br/>

![Clientsetup](https://user-images.githubusercontent.com/129562058/229271758-646ae6c3-0b59-407e-b6df-ee14dfbaa97e.png)<br/><br/>

Open the CLIENT1 vm, select and open the Windows ISO file (mimic step 4)<br/>
Go through windows 10 setup as prompted (select I do not have a product key) (install windows 10 pro)<br/>

After successfully setting up a Windows 10 account -> Check if the internet is working with the cmd prompt using ipconfig<br/>And ping to google.com<br/>

![ipconfig check](https://user-images.githubusercontent.com/129562058/229272795-0887447a-271b-4240-9279-4a9dfd9bdcc4.png)<br/><br/>

To also demonstrate that CLIENT one is connected to the DC you can ping the domain controller in cmd prompt <br/>
![pingdomain contrllr](https://user-images.githubusercontent.com/129562058/229273112-76d342b4-a5c5-4e54-a3b8-4f40e06f664f.png)<br/><br/>

Rename the PC by right clicking the start button -> selecting system -> rename this PC advanced (to join the domain) -> Change -> under "member of" select domain and type in whatever name you gave your domain <br/><br/>

![rename this pc advanced](https://user-images.githubusercontent.com/129562058/229273284-5dcbb616-8e67-4b8e-ae99-f5834cc51c5a.png)<br/><br/>

Finally head back to the DC and open up the Active directory Users and Computers and the DHCP window to verify that there is a connection between the DC and CLIENT1 VMs<br/><br/>

![proof](https://user-images.githubusercontent.com/129562058/229273843-01eb6692-1cf7-4a23-8a65-29a45532b7aa.png)<br/><br/><br/><br/>

now if you want you can go onto the CLIENT1 vm and use one of the 1000 users that were created previously to log in.

<br />
<br />

</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
