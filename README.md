# Active-directory-home-lab
<h1> Description </h1>
Creating an 'Active Directory Home Lab using VirtualBox' involves setting up a simulated network environment on your personal computer using VirtualBox virtualization software. The purpose of this lab is to gain practical knowledge and hands-on experience with Active Directory and Windows networking concepts.
<br />
<br />
By configuring and running this lab, you will be able to thoroughly understand how Active Directory functions and grasp broader Windows networking principles. This learning process includes creating and managing domain controllers, user accounts, groups, organizational units, and group policies within the Active Directory infrastructure. 
<br />
<br />
This home lab setup is intended to provide a safe and isolated environment, enabling you to experiment freely without affecting your actual production systems. Through hands-on practice, you can explore various Active Directory scenarios and simulate network interactions, thereby enhancing your comprehension of Windows networking in general. 
<br />
<br />
<h2>Necessory Downloada and Utilities Used</h2>

- <a href="https://www.virtualbox.org/wiki/Downloads"><b> Oracle virtualbox </b></a>
- <a href="https://www.microsoft.com/en-us/software-download/windows10ISO"><b> Windows 10 ISO </b></a>
- <a href="https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019"><b> Server 2019 ISO </b></a>

<br />
<br />
<h2>High level overview and visualization</h2> <br/>
<img src="https://i.imgur.com/s06cmka.png" height="80%" width="80%" alt="installation steps"/>
<br />
<br />
<h3>Overview of the Above Diagram/Flow Chart:</h3>
In this project, we will be creating an Active Directory home lab using Oracle VirtualBox, a virtualization platform for running virtual machines. The first step is to download and install Oracle VirtualBox, which will serve as the host for our virtual machines. Once installed, we will obtain Windows 10 and Windows Server 2019 ISO files, which we will use to install the two operating systems on separate virtual machines.

After completing the software installations, we will proceed to create the first virtual machine, which will act as the domain controller hosting Active Directory. To ensure connectivity, this virtual machine will have two network adapters—one for external internet access and the other for the private VirtualBox network, to which the clients will connect.

Once the domain controller virtual machine is set up, we will install Windows Server 2019 on it and assign IP addressing for the internal network. The external network will automatically obtain IP addressing from the home router, alleviating any configuration concerns.

Following this, we will name the server, install Active Directory, and create the domain. To enable internet access for clients on the private network, we will configure Network Address Translator (NAT) and Remote Access Service (RAS) on the domain controller.

Next, we will set up a DHCP (Dynamic Host Configuration Protocol) on the domain controller. This will allow the Windows 10 client machine, which we will create later, to obtain an IP address automatically.

Before creating the client virtual machine, we will run a PowerShell script that automatically generates a thousand users within Active Directory. This will facilitate testing and experimenting with various scenarios in a realistic environment.

After creating the users, we will proceed to set up another virtual machine and install Windows 10 on it. This virtual machine, named 'Client1,' will be connected to the private VirtualBox network and then joined to the domain. Finally, we will log into 'Client1' using one of the domain accounts.

Throughout this project, we will document each step, ensuring clarity and accuracy in our descriptions. By adding these instructions and experiences to GitHub, we aim to share our knowledge and enable others to understand the intricacies of setting up an Active Directory home lab using VirtualBox, as well as grasp the fundamentals of Windows networking in a practical and hands-on manner.
<br />
<br />
<h2>Program walk through</h2>
<h3>Creation of Windows Server 2019 ISO in VirtualBox:</h3></br></br>
To set up a Windows Server 2019 virtual machine in VirtualBox, we'll begin by creating a new virtual machine. We'll name it 'DC' for Domain Controller, choose 'Other Windows (64-bit)' as the operating system, and allocate 2048 megabytes (2GB) of RAM. The disk will be created with the default settings.

Before installing Windows Server 2019, let's adjust some settings. In the 'Settings' menu, under 'Advanced,' we'll set both 'Share Clipboard' and 'Drag and Drop' to 'Bi-directional' to enable easy file sharing between the host computer and the virtual machine.

Next, we'll navigate to the 'System' settings and configure the processor based on our CPU type. Then, in the 'Network' section, we'll add another adapter. The first adapter will be used for internet access and will be set to 'NAT,' while the second one will connect to the internal VMware network.

With the virtual machine configured, we'll start it by double-clicking it and then select the downloaded Server 2019 ISO. Once the ISO is added, we'll start the virtual machine, which might take some time.

After the virtual machine starts up, we'll proceed with the installation process. We'll choose the 'Standard' option with 'Desktop Experience' to have a graphical user interface (GUI) on the server. We'll accept the license agreements and proceed with a custom installation.

During the installation, it will prompt us to set up a password, and the default ID will be 'Administrator,' which we must remember.

Once the installation is complete, our Windows Server 2019 virtual machine will be up and running in VirtualBox. This virtual machine will serve as our Domain Controller and will host Active Directory, allowing us to simulate and experiment with various Windows networking scenarios in a safe and isolated environment.
</br></br>
<img src="https://i.imgur.com/kJ23hzB.png" height="80%" width="80%" alt="installation steps"/>
</br></br>
<b>Note: For opening the Ctrl+Alt+Delete screen in VirtualBox, you cannot directly type the keys on the virtual machine. Instead, follow these steps:

- Click on the virtual machine to ensure it has focus.
- Navigate to the top menu of the VirtualBox window and select "Insert" from the menu options.
- From the "Insert" dropdown menu, choose the "Insert Ctrl+Alt+Delete" option.</br>

By following these steps, you can successfully open the Ctrl+Alt+Delete screen on your VirtualBox virtual machine. This screen is essential for performing certain actions, such as changing passwords or accessing task manager, within the virtual environment.</b> </br></br>

 
<h3><b>Setting up IP Addresses for the Network Interface Cards (NIC) on Domain Controller:</b></h3>

After successfully installing Windows Server 2019, the next step is to manually assign an IP address for the internal network NIC, as the internet NIC will automatically receive an IP address from the home router.

To set up the IP address for the internal NIC, follow these steps:

1. Click on the network icon in the taskbar, then click "Network and Internet Settings," and select "Change Adapter Options."

2. In the "Network Connections" window, identify the two network adapters. To distinguish between them, right-click on one of the NICs, select "Status," and check the details. If it shows an IPv4 address starting with "10.0.something," it is the internet NIC. If it displays "Autoconfiguration IPv4," it is the internal NIC. Rename them accordingly, using labels like "_INTERNET_" and "X_Internal_X."</br></br>
<img src="https://i.imgur.com/BkT4Pto.png" height="80%" width="80%" alt="installation steps"/></br></br>
3. Now, right-click on the internal network adapter and choose "Properties." Click on "IPv4" and select "Use the following IP address."

4. Assign the IP address "172.16.0.1" to the IP Address column, set the Subnet Mask to "255.255.255.0," and leave the Default Gateway empty, as the domain controller itself acts as the default gateway.

5. For the Preferred DNS server, enter the domain controller's own IP address or use the loopback address "127.0.0.1." Click "OK" to apply the changes.</br></br>
<img src="https://i.imgur.com/AK9utjZ.png" height="80%" width="80%" alt="installation steps"/></br></br>
6. Finally, to rename the PC as "DC" (Domain Controller), right-click on the Start menu, select "System," and click on "Rename this PC." Enter "DC" as the name, click "Next," and then restart the system to apply the changes.</br></br>

<h3><b>Installing Active Directory Domain Services:</b></h3></br>

To begin installing Active Directory Domain Services on Windows Server 2019, follow these steps:

1. Open "Server Manager" and click on "Add Roles and Features." Proceed through the wizard until you reach the "Select server roles" section.

2. Check the box for "Active Directory Domain Services" and continue clicking "Next" until the "Install" button appears. Click on "Install" to begin the installation process. Once completed, click "Close."</br></br>
<img src="https://i.imgur.com/oHKrDML.png" height="80%" width="80%" alt="installation steps"/></br></br>
3. To create a domain, click on the warning symbol '⚠️' in Server Manager and select "Promote this server to a domain controller." This opens the "Active Directory Domain Services Configuration Wizard."

4. Choose the option to "Add a new forest" and specify the root domain address as 'mydomain.com.' Proceed by clicking "Next."</br></br>
<img src="https://i.imgur.com/xye89Ai.png" height="80%" width="80%" alt="installation steps"/></br></br>
5. Set the password for the Directory Services Restore Mode (DSRM) using a secure password like "Password1." Continue clicking "Next" until the "Install" button becomes available. The system will automatically restart after installation.

6. After the restart, log in again using the built-in administrator password. You will notice the login format has changed to 'MYDOMAIN\ADMINISTRATOR' to indicate the domain.

7. To create a dedicated domain admin account, access "Active Directory Users and Computers" from the Windows Administrative Tools in the Start menu.

8. Right-click on 'mydomain.com' and select "New" to create an Organizational Unit (OU). Name it '_ADMINS' and click "OK."

9. Within the '_ADMINS' OU, right-click and select "New," then choose "User." Provide the first name, last name, and in the user logon name field, use 'first initial and last name' (e.g., 'a-br' for 'John Smith'). Set the password (e.g., "Password1") and check "Password never expires." Proceed by clicking "Next" and then "Finish" to create the user.

10. To make this user a domain admin, right-click on the user, go to "Properties," and click on the "Member Of" tab. Add "Domain Admins" as a member and click "Check Names," then click "OK," "Apply," and "OK."</br></br>
<img src="https://i.imgur.com/jS5pGbG.png" height="80%" width="80%" alt="installation steps"/></br></br>
11. Now, log out from the current account and choose the "Other account" option to log in using the newly created domain admin account. Use the ID and password we set earlier, and you will log in as the domain admin.</br></br>
<h3><b>Installing Remote Access Server (RAS) and Network Address Translation (NAT) on Windows Server 2019:</b></h3></br></br>

The purpose of installing RAS and NAT is to enable Windows 10 clients, created within the private virtual network, to access the internet through the domain controller. Follow these steps to set up RAS and NAT:

1. Open "Server Manager" and click on "Add Roles and Features." Continue clicking "Next" until the "Select server roles" section.

2. Check the box for "Remote Access" to add this feature. Click "Next" and then add the "Routing" feature. Proceed by clicking "Next" until the "Install" button appears. Click on "Install" to start the installation process, and then close the window.

3. Access "Tools" in Server Manager and click on "Routing and Remote Access."

4. In the "Routing and Remote Access" window, right-click on "DC(local)" and select "Configure."

5. In the configuration window, click "Next" to proceed. Select the "Network Address Translation (NAT)" tool, which enables internal clients to connect to the internet using a single public IP address.</br></br>
<img src="https://i.imgur.com/E3IWNjG.png" height="80%" width="80%" alt="installation steps"/></br></br>
6. Click "Next" and then choose the option "Use the public interface to connect to the internet." From the drop-down list, select the interface named "INTERNET" as the network interface responsible for connecting to the internet.</br></br>
<img src="https://i.imgur.com/TnenYLk.png" height="80%" width="80%" alt="installation steps"/></br></br>
7. Click "Next" and then "Finish" to complete the NAT configuration.

By setting up RAS and NAT in this manner, the private virtual network clients, including the Windows 10 client, will have access to the internet through the domain controller.</br></br>
<h3><b>Setting up DHCP Server:</b></h3>.</br>

Dynamic Host Configuration Protocol (DHCP) is a network server that automatically provides and assigns IP addresses, default gateways, and other network parameters to client devices. In this setup, DHCP will allow Windows 10 clients to receive IP addresses that enable them to access the internet even though they are in the private network.

To set up DHCP on Windows Server 2019, follow these steps:

1. Open "Server Manager" and click on "Add Roles and Features." Continue clicking "Next" until the "Select server roles" section.

2. In the roles section, select "DHCP Server" and click on "Add Feature." Click "Next" until the "Install" option appears. Click on "Install" to begin the installation process and then close the window.

3. Access "Tools" in Server Manager and click on "DHCP."

4. Right-click on "IPv4" and select "New Scope." Click "Next" to proceed.

5. Name the scope as "172.16.0.100-200" and click "Next."</br></br>
<img src="https://i.imgur.com/us8Emx6.png" height="80%" width="80%" alt="installation steps"/></br></br>
6. Add the start IP address as "172.16.0.100" and the end IP address as "172.16.0.200." Set the subnet mask to "24" for a length of /24.</br></br>
<img src="https://i.imgur.com/s7llxX0.png" height="80%" width="80%" alt="installation steps"/></br></br>
7. Continue clicking "Next" until the "Gateway" option comes up. Add the domain controller's IP address, which is "172.16.0.1," and click "Add" and then "Next."</br></br>
<img src="https://i.imgur.com/ZbxpcJw.png" height="80%" width="80%" alt="installation steps"/></br></br>
8. Keep clicking "Next" until you reach the "Finish" button. Click "Finish" to complete the scope creation.

9. Now, right-click on "dc.mydomain.com" (your domain controller) and click "Authorize." Then, refresh the DHCP console.

By following these steps, you have successfully set up the DHCP server on Windows Server 2019. This DHCP server will automatically provide IP addresses to Windows 10 clients in the private network, allowing them to browse the internet. </br></br>
<h3><b>Configuring the Local Server:</b></h3></br>

In our lab environment, we can configure the local server, the domain controller, to allow browsing the internet. This configuration is typically not recommended for production environments, but it serves our educational purposes in the lab.

To perform this configuration, follow these steps:

1. Click on "Configure this local server" in Server Manager to access the server's settings.

2. In the Local Server settings, locate and click on "IE Enhanced Security Configuration."

3. In the "IE Enhanced Security Configuration" window, you'll find settings for both administrators and users. Turn off the enhanced security configuration for both "Administrators" and "Users."</br></br>
<img src="https://i.imgur.com/Wdh0LU0.png" height="80%" width="80%" alt="installation steps"/></br></br>
By disabling the Internet Explorer Enhanced Security Configuration for administrators and users on the domain controller, you allow the server to browse the internet freely. Keep in mind that this configuration should only be applied in lab environments and not in production settings where security measures need to be more stringent.</br></br>
<h3><b>now downlad the powershell script named "1_CREATE_USER" and plain text file named "names" file  given above above </b></h3>
<h3><b>Creating Users Using PowerShell Script:</b></h3></br>

To create users using a PowerShell script, follow these steps:

1. First, ensure you have a "names" file containing the names of the users you wish to create. Add your desired names to this file.

2. Click on the Start menu, search for "Windows PowerShell," and right-click on "Windows PowerShell ISE." Select "Run as administrator" to open PowerShell with elevated privileges.

3. In the PowerShell ISE, click on "File" and choose the script named "1_CREATE_USER" (presumably the script for user creation).

4. Before running the script, you need to enable the execution of all scripts. To do this, run the command "Set-ExecutionPolicy Unrestricted" in the PowerShell command line and press Enter. If prompted with a confirmation pop-up, click "Yes to all" to allow unrestricted script execution.

5. Now, navigate to the location where the "names" file (with plain text names) is stored. You can use the "cd" (Change Directory) command to move to the appropriate location.

6. Click the "Run" button at the top of the PowerShell ISE to execute the script. The script will import the Active Directory module and begin creating the specified users based on the names provided in the "names" file.
<img src="https://i.imgur.com/A5kgrMf.png" height="80%" width="80%" alt="installation steps"/></br></br>
<h3><b>Creating a Windows 10 Client:</b></h3></br>

To create a Windows 10 client in VirtualBox, follow these steps:

1. Open VirtualBox and click on "New." Provide a name for the virtual machine, such as "CLIENT1," and choose "Windows 10 (64-bit)" as the operating system. Click "Continue."

2. Adjust the RAM size to 4096 MB (4GB) and continue by clicking "Continue."

3. Create a virtual hard disk and continue through the prompts until the process is complete. Once finished, close the window.

4. Access the settings of the "CLIENT1" virtual machine. In "Advanced," change the option to "Bidirectional" for improved functionality.

5. In the "System" settings, consider increasing the allocated processing power slightly for better performance.

6. Change the network settings to "Internal Network" to connect the virtual machine to the internal network of the VirtualBox environment. Click "OK" to save the changes.

7. Double-click on the "CLIENT1" virtual machine to start it. Click on "Add" and select the Windows 10 ISO file to add it to the virtual machine.

8. Complete the setup by choosing "Windows 10 Pro" and the desired minimal experience during the installation process.

With these steps completed, your Windows 10 client machine is now ready to be used within VirtualBox. It is a fully functional virtual machine that can be utilized for various purposes, including testing software, browsing the internet, and exploring Windows 10 features. </br></br>

<h3><b>Testing the Features of Windows 10 Client:</b></h3>

After creating and configuring the Windows 10 client in VirtualBox, it's essential to test its functionality to ensure it's correctly utilizing the features we've set up.

1. To verify internet connectivity, open a command prompt on the Windows 10 client and ping "google.com." If the ping is successful, it confirms that the client can access the internet.

2. Next, ping "mydomain.com" from the command prompt. If the ping is also successful, it indicates that the internal client is successfully using the internet through the domain controller (DC).

By conducting these tests and observing positive results, we can confirm that the Windows 10 client is properly configured to use the internet through the domain controller. This means that the client can access both external (internet) resources and internal (mydomain.com) resources through the network settings and services we have set up in our Active Directory home lab.</br></br>
<img src="https://i.imgur.com/umKv6kw.png" height="80%" width="80%" alt="installation steps"/></br></br>

<h3><b>Renaming the Windows 10 PC:</b></h3>

To rename the Windows 10 PC and join it to the domain, follow these steps:

1. Right-click on the Start menu and select "System."

2. In the "System" settings, click on "Rename this PC" and then click on "Advanced system settings."

3. In the "System Properties" window, click on the "Change" button to rename the PC.

4. Enter the desired name for the PC, such as "CLIENT1," and click "OK."

5. To join the PC to the domain, click on the "Domain" button and add the domain name, which is "mydomain.com" in this case.
<img src="https://i.imgur.com/DPMJOrd.png" height="80%" width="80%" alt="installation steps"/></br></br>
6. Provide the ID and password for the domain admin account to authenticate the PC to the domain. Click "OK" and then "Close."

7. Finally, restart the PC by clicking "Restart Now" to apply the changes.

After completing these steps, the Windows 10 PC will be renamed to "CLIENT1" and successfully joined to the domain "mydomain.com." The PC is now a member of the domain and can access domain resources and services, contributing to a fully functional Active Directory home lab environment.

<h3><b>Address Leases in Server 2019:</b></h3>

Address leases refer to the dynamic assignment of IP addresses to client devices by the DHCP (Dynamic Host Configuration Protocol) server. When a client device, such as the Windows 10 PC (CLIENT1), is connected to the network and joined to the domain, it requests an IP address from the DHCP server. The server then assigns an IP address to the client for a specific lease duration.

To view address leases in Windows Server 2019, follow these steps:

1. Click on "Tools" and select "Address Manager."

2. In the Address Manager, choose "DHCP" from the options.

3. In the DHCP section, expand "dc.mydomain.com," then "IPv4," and finally "Scope."

4. Click on "Address Leases." Here, you will see the list of leased IP addresses assigned to client devices.

5. You should see one lease, which was obtained from the client computer (e.g., CLIENT1) after renaming and joining it to the domain.</br></br>
<img src="https://i.imgur.com/cDyi5oS.png" height="80%" width="80%" alt="installation steps"/></br></br>
6. To verify the client's presence in Active Directory, open "Active Directory Users and Computers." Navigate to "mydomain.com" and then "Computers." You will find "CLIENT1" listed as a joined device under this organizational unit.</br></br>
<img src="https://i.imgur.com/0gysiye.png" height="80%" width="80%" alt="installation steps"/></br></br>
By monitoring address leases in the DHCP server, administrators can track the dynamically assigned IP addresses to various client devices. This ensures efficient utilization of IP addresses and helps in managing network resources effectively. Additionally, verifying the presence of the client in Active Directory provides confirmation that the Windows 10 PC (CLIENT1) has successfully joined the domain and is ready for domain-based network operations.</br></br>
<h3><b>Logging in Using Created Accounts from Domain Controller that Created By PowerShell </b></h3>

To login using a random user account created on the domain controller, follow these steps:

1. On the Windows 10 client (CLIENT1), click on "Other users" during the login process.

2. In the login options, select "Sign in to mydomain."

3. Choose a random user ID from the list of domain user accounts created earlier.

4. Enter the default password that has been assigned to all user accounts for the initial login.

5. After entering the credentials, the system will proceed to log in with the specified user account.

By using this process, you can test the functionality of the user accounts created on the domain controller. The ability to successfully log in using different user accounts confirms that the Active Directory domain is properly functioning, and user authentication is working as expected.</br></br>

<h2><b>Conclusion:</b></h2>

This project of setting up an Active Directory home lab using VirtualBox has been a valuable learning experience, providing insights into various aspects of Windows networking and Active Directory management. Throughout the project, we have learned and achieved the following:

1. **VirtualBox Configuration:** We started by installing and configuring VirtualBox, which allowed us to create and manage virtual machines for our lab environment.

2. **Windows 10 and Server 2019 Installation:** We downloaded and installed Windows 10 and Server 2019 ISOs, setting up separate virtual machines for the client and domain controller.

3. **Active Directory Domain Services:** We successfully installed and configured Active Directory Domain Services, creating a domain controller (DC) and setting up a domain named "mydomain.com."

4. **Network Configuration:** We set up network interfaces for the DC, allowing it to connect to the internet (NAT) and the private virtual network for clients (Internal Network).

5. **DHCP and RAS/NAT Configuration:** We enabled DHCP to automatically assign IP addresses to clients, and we set up RAS/NAT to allow clients to access the internet through the DC.

6. **User Creation and Management:** We used PowerShell scripts to create multiple users in Active Directory and set up a dedicated domain admin account. This helped us explore user management capabilities.

7. **Windows 10 Client Setup:** We created a Windows 10 client virtual machine, joined it to the domain, and verified its successful integration with Active Directory.

8. **Testing and Verification:** We verified network functionalities by testing internet connectivity and domain-based login using PowerShell.

Through this project, we gained practical knowledge of Active Directory, domain configuration, user management, DHCP, RAS/NAT, and virtualization using VirtualBox. We also learned how to troubleshoot and test different components of our lab setup.

Additionally, by documenting our entire project on GitHub, we have shared our experiences and learnings with others in the tech community. This contribution will help aspiring IT professionals, system administrators, and networking enthusiasts understand the process of setting up and managing an Active Directory home lab using VirtualBox, fostering collaborative learning and knowledge-sharing in the broader community.

Overall, this project has not only enhanced our understanding of Active Directory and Windows networking but has also equipped us with valuable skills applicable to real-world scenarios in managing and maintaining network environments.
