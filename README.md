<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Installing Active Directory in Azure</h1>
This lab outlines the process of installing Active Directory (AD) within an Azure environment. The setup forms the foundation for future labs that will build on this configuration. The lab utilizes two virtual machines (VMs) hosted on Azure, both of which reside on the same virtual network (VNet). This particular exercise focuses on one VM, which will be configured as the domain controller with Active Directory installed. The second VM will be configured as a client and will join the domain in subsequent labs.



</p>
<br />

<h2>Environments and Technologies Utilized</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop (RDP)
- Active Directory Domain Services (AD DS)



</p>
<br />

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)



</p>
<br />

<h2>Installation Process</h2>
Before utilizing the virtual machines (VMs), it is crucial to set the IP address of the domain controller VM to static. By default, VMs with dynamic IPs, even if they are on the same virtual network (VNet), may fail to communicate with each other. Without this change, the client VM will not be able to join the domain that will be established later.
</p>
To configure the static IP in the Azure portal, navigate to the Networking tab of the domain controller VM. Select the Network Interface and open the IP configurations tab. Toggle the IP assignment to "Static" and save your changes. This step ensures that the domain controller retains a consistent IP address, which will be referenced in subsequent configurations.
<br />

![image](https://github.com/user-attachments/assets/069441af-9e8b-499f-939f-d3651dacf54d)

</p>
</p>
<br />
<br />

After configuring the static IP, log in to the client VM to verify connectivity to the domain controller. Run the command ping -t <domain controller private IP address>. Initially, you may notice that the ping request times out. This occurs because ICMPv4 is not enabled on the domain controller's local Windows Firewall.
![image](https://github.com/user-attachments/assets/2b73d677-a275-4c38-ac11-7e73bfcdcd2c)

</p>
</p>
</p>
<br />
<br />
To resolve this, access the domain controller VM, open the search bar, and type wf.msc to launch the Windows Defender Firewall with Advanced Security. Navigate to the Inbound Rules and enable the "Core Networking Diagnostics - ICMP Echo Request" rules. Returning to the client VM, you should now see successful ping responses, confirming connectivity.

![image](https://github.com/user-attachments/assets/6f69d402-e151-45e6-be8e-6e0ab9e67855)
![image](https://github.com/user-attachments/assets/4802c720-c243-4dd2-b5fd-07474cbf0dc7)

</p>
<p>
<br />
<br />
Next, proceed with the installation of Active Directory on the domain controller VM. Open Server Manager, select "Add Roles and Features," and click Next. Confirm the private IP address of the domain controller VM. In the Server Roles section, select "Active Directory Domain Services," click "Add Features," and then click Next, followed by Install.

Once the installation is complete, the server needs to be promoted to a domain controller. In Server Manager, click on the warning flag in the top right corner, and select "Promote this server to a domain controller." Choose "Add a new forest" and specify the domain name (e.g., mtrantest.com). Set a password for the domain and proceed through the setup screens by clicking Next, then Install.
![image](https://github.com/user-attachments/assets/2b30b9a5-8b54-4893-ae39-cfcd49d4e614)



</p>
<br />

<h2>Important Login Information</h2>

When logging back into the domain controller VM via Remote Desktop Connection, it is important to log in with the domain context. This involves typing the domain path followed by the username. For example, mydomain.com\labuser. In this case, it would be mtrantest.com\labuser. With Active Directory now installed, additional configurations can be implemented in future labs, and the client VM will be able to join the newly created domain.
