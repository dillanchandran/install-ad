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

<p>
7. Now that Active Directory is installed, the domain controller virtual machine can be promoted to function as an actual domain controller. Log in to the domain controller virtual machine using Remote Desktop/Windows App. Open "Server Manager", and look for the notification flag in the upper-right corner. Click the flag, select "Promote this server to a domain controller" and a configuration wizard will pop up.
</p>
<p>
<img width="500" alt="Screen Shot 2025-01-05 at 1 11 13 PM" src="https://github.com/user-attachments/assets/755516e6-a75a-467c-b6c0-0248330d475b" />
</p>
<br />

<p>
8. In the Active Directory Domain Services Configuration Wizard, select the deployment operation as "Add a new forest" since you are creating a new domain. Enter your desired root domain name (e.g. mydomain.com) and click Next. Complete the rest of the wizard, reviewing and confirming the default options. Click Install to finalize the configuration and promote the server to a domain controller. The VM will restart automatically after the installation is complete.
</p>
<p>
<img width="500" alt="Screen Shot 2025-01-05 at 1 12 42 PM" src="https://github.com/user-attachments/assets/ea8e59ee-495d-4ce5-aed1-0a6367e36ac6" />
</p>
<br />

<p>
9. When logging back into the domain controller virtual machine, you will need to include your domain name followed by a backslash before your username (e.g. mydomain.com\user) because the virtual machine is now part of the domain. Once logged in, open Active Directory Users and Computers. In the domain, create two new Organizational Units:<br />

- _EMPLOYEES<br />

- _ADMINS
</p>
<p>
<img width="600" alt="Screen Shot 2025-01-05 at 1 21 30 PM" src="https://github.com/user-attachments/assets/63714641-3d03-4ae1-b417-2f1037794e49" />
<img width="200" alt="Screen Shot 2025-01-05 at 1 22 21 PM" src="https://github.com/user-attachments/assets/19520c22-329f-4b3b-84cb-36288a8f74c8" />
</p>
<br />

<p>
10. In the" _ADMINS" Organizational Unit , create a new user. Right-click the "_ADMINS", hover over New, and select User. Fill out the User form with the desired details. Once the user is created, right-click the user and select Properties. In the Properties window, navigate to the Member Of tab. Click Add, type Domain Admins, and select it to add the user to the Domain Admins group. Click Apply and OK to save the changes.
</p>
<p>
<img width="400" alt="Screen Shot 2025-01-05 at 1 26 02 PM" src="https://github.com/user-attachments/assets/97623853-8d69-4b6d-b3a3-0c2394c8d91f" />
</p>
<br />

<p>
11. Now log out of the domain controller virtual machine and login tothe client virtual machine. When logged in open System settings click "Advanced system settings", you may need to scroll down. On system properties select "Change", under "Member of" select "Domain" and type the domain of your domain controller. Click "OK" and enter the login details of the Domain Admin created in the previous step. You will be prompted to restart the client virtual machine to complete the process. Accept the prompt and allow the virtual machine to restart. The client virtual machine is now a member of the domain.
</p>
<p>
<img width="500" alt="Screen Shot 2025-01-05 at 1 32 52 PM" src="https://github.com/user-attachments/assets/09da2190-059a-49ef-bfd4-1ebf5c768585" />
<img width="300" alt="Screen Shot 2025-01-05 at 1 34 18 PM" src="https://github.com/user-attachments/assets/c8c6b9ef-637e-4af9-bed5-fe96ca4aecdd" />
<img width="300" alt="Screen Shot 2025-01-05 at 1 35 15 PM" src="https://github.com/user-attachments/assets/ab005cb7-70cc-42dd-81fa-5764b6ca4e41" />
</p>
<br />


<p>
12. Log back into the client virtual machine as the domain controller admin, open the system settings and select "Remote Desktop", you may need to scroll down. Then select "Select uses tha can remotely access this PC". Add he group "Domain Users" and click "OK".
</p>
<p>
<img width="400" alt="Screen Shot 2025-01-05 at 3 39 26 PM" src="https://github.com/user-attachments/assets/10cb9b6e-00fa-470a-bfa2-fede19d0248e" />
<img width="400" alt="Screen Shot 2025-01-05 at 3 39 59 PM" src="https://github.com/user-attachments/assets/63854463-3897-4749-bf3c-5c359813d607" />
<img width="200" alt="Screen Shot 2025-01-05 at 3 51 16 PM" src="https://github.com/user-attachments/assets/527f8a24-27a2-4082-8ff8-5ec9fd238314" />

</p>
<br />

Now all users that you create in the "_EMPLOYEES" Organizatitonal Unit will now be able to login to the client virtual machine we have connected to our domain controller. All resources neeeded to be shred amongst all users can now be managed within a centralized domain environment.










<br />
