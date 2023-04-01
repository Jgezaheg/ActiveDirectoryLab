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
Install Virtual box and Create a VM named DC (Domain Controller):</h3> <br/>
 
![CreateVM-DomainController](https://user-images.githubusercontent.com/129562058/229259180-4d677f7c-b152-4740-9a2f-21fc810c9db5.png)<br/>
 
 <h3><p align="center">
 Assign 2 GBs of RAM and 4 cpu cores to the VM:</h3> <br/>
 
![Setting up memory and processing for VMs](https://user-images.githubusercontent.com/129562058/229258595-4db76e04-6d4a-4c35-9190-4965d01c46ae.png)<br/>

<h3><p align="center">
 Set up 2 NIC adapters:</h3> <br/>
 
 ** Adapter 1: dedicated to the internet = NAT**<br/>
 ** Adapter 2: dedicated to internal VM ware Network = Intnet**
 
 ![adding NICs to vm one-NAT-internet](https://user-images.githubusercontent.com/129562058/229260101-8673cd67-2cb7-4030-8d03-fbc370562333.png)
 ![adding NICs to vm two-internal-intnet](https://user-images.githubusercontent.com/129562058/229260057-e02442fe-38d0-4195-97ca-cb5f30703d0e.png)<br/>
 
<h3><p align="center">
Open the DC vm, select and open the Server 2019 ISO file :</h3> <br/>

![Sever2019ISO_to_DC](https://user-images.githubusercontent.com/129562058/229259459-cde0330d-38ca-4cc7-b267-c1b2452610f9.png)<br/>

<h3><p align="center">
Setup Windows Sever 2019 follow instructions presented on the screen :</h3> <br/>
**select either desktop experience OS during set up for GUI**
**Dont press any keys during reboot process**

![WinServer2019 Setup](https://user-images.githubusercontent.com/129562058/229259560-d55eb2a4-b47c-4d77-86b2-b8f9f7f12291.png)<br/>

<h3><p align="center">
Setup IP Addressing :</h3> <br/>
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
Install Active Directory Domain Services (AD DS) :</h3> <br/>
Navigate to the Server Manager:<br/>
**Select add roles and features -> select next -> select next -> select the server named DC (only one should be available) -> under server roles select "active directory domain services" -> add roll -> select next until prompted to install.<br/>

![installingADDS](https://user-images.githubusercontent.com/129562058/229262262-02f7dc7c-450c-46a0-a0fb-735f5517588d.png)<br/>

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
