<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>


<p>
1. In the Azure Portal, create two virtual machines within the same virtual network (VNet):

- One VM running Windows 10 (22H2), which will act as a client machine.
- One VM running Windows Server 2022, which will serve as the domain controller.

2. Once both virtual machines have been deployed, navigate to the IP Configurations section in the Network Interface Card (NIC) settings of the domain controller VM.

- Change the IP address assignment from dynamic to static to ensure the domain controller has a consistent private IP address for reliable network communication.
</p>
<p>
<img width="600" alt="Screen Shot 2025-01-05 at 12 36 14 PM" src="https://github.com/user-attachments/assets/49db1c61-e190-4bb9-a6f5-8fdd00357407" />
</p>
<br />

<p>
 
3. Connect to the domain controller VM via Remote Desktop/Windows App, using its public IP. When logged in, open the "Windows Defender Firewall with Advance Security" program and select "Windows Defender Firewall Properties". Turn off the firewall state on the "Domain Profile", "Private Profile" and "Public Profile" tabs. Click Apply and then OK to save your changes.

</p>
<p>
<img width="400" alt="Screen Shot 2025-01-05 at 12 43 25 PM" src="https://github.com/user-attachments/assets/ed278f4b-7228-4321-b0e5-0cadbe4e050b" />
<img width="250" alt="Screen Shot 2025-01-05 at 12 43 52 PM" src="https://github.com/user-attachments/assets/c66e2252-6a45-44ee-b37b-cef9a8e9abdf" />
</p>
<br />

4. In the Azure Portal, navigate to the Network Interface Card (NIC) settings of the domain controller virtual machine and copy its Private IP address.


</p>
<p>
<img width="500" alt="Screen Shot 2025-01-05 at 2 08 21 PM" src="https://github.com/user-attachments/assets/08b1294b-fdc6-49ec-8841-ed33afaad737" />

</p>

<br />

5. Go to the client virtual machine's NIC settings in the Azure Portal. Under the DNS server configuration, change the setting from "Inherit from Virtual Network" to "Custom", and paste the Private IP address of the domain controller as the client’s DNS server. Save the changes to ensure the client VM can communicate with the domain controller effectively.

</p>
<p>
<img width="500" alt="Screen Shot 2025-01-05 at 2 11 12 PM" src="https://github.com/user-attachments/assets/cf454c9f-0191-4bea-989a-99261ed9b403" />
</p>

<br />

6. Log in to the Domain Controller virtual machine via Remote Desktop/Windows App. Open "Server Manager" and click on "Add Roles and Features". Follow the wizard steps, selecting the default options until you reach the "Select Server Roles" screen. Check the box for Active Directory Domain Services. Continue through the wizard, reviewing and confirming the required features, and then click Install. Wait for the installation to complete and restart the server when installation is complete.

</p>
<p>
<img width="400" alt="Screen Shot 2025-01-05 at 1 09 04 PM" src="https://github.com/user-attachments/assets/38b5d22b-6308-4ed5-a459-fff811d7639e" />
</p>

Congratulations, Active Directory has now been installed!









<br />
