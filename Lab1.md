# Lab: Installing and configuring Server Core

**Scenario**
Your team in the IT department at Adatum Corporation just purchased a new server that has no operating system. The team decides to install Windows Server 2016 Datacenter Evaluation in Server Core mode to test Server Core functionality. Your task is to perform the installation and configuration of this server. You will name it **LON-SVR6**, give it a static IP address of 172.16.0.26, and join it to the Adatum.com domain with all other default settings.

**Objectives**
After completing this lab, you will be able to:
* Install the Server Core option for Windows Server 2016.
* Configure Server Core. 

**Lab Setup**
- Estimated Time: 60 minutes
- Virtual machines: **20740C-LON-DC1** and **20740C-LON-SVR6**
- User name: **Adatum\Administrator**
- Password: **Pa55w.rd**
- 
- For this lab, you will use the available virtual machine environment. Before you begin the lab, complete the following steps:
1. On the host computer, start **Hyper-V Manager**.
2. In Hyper-V Manager, click **20740C-LON-DC1**, and then in the **Actions** pane, click **Start**.
3. In the **Actions** pane, click **Connect**. Wait until the virtual machine starts.
4. Sign in by using the following credentials:
   1. User name: **Adatum\Administrator**
   2. Password: **Pa55w.rd**
5. In Hyper-V Manager, right-click **20740C-LON-SVR6**, and then select **Connect**. In the virtual machine connection window, click **Media**, point to **DVD Drive**, and then click **Insert Disk**.
6. Browse to **D:\Program Files\Microsoft Learning\20740\Drives**, select **WinServer2016_1607.iso**, and then click Open. 


## Exercise 1: Installing Server Core

**Scenario**
You determine that Server Core offers you the best installation option and decide to evaluate a server that uses Server Core. The main task for this exercise is as follows: 1.Install Windows Server 2016 Datacenter Evaluation on LON-SVR6. 

**Task 1: Install Windows Server 2016 Datacenter Evaluation on LON-SVR6**
1. In the **20740C-LON-SVR6 Virtual Machine Connection** window, click the **Start** icon.
2. When **LON-SVR6** starts Windows Setup, install **Windows Server 2016 Core** by using the **Windows Server 2016 Datacenter Evaluation** option.
3. Use the Custom option rather than the Update option, accepting all other default values and license agreements.
4. Use Pa55w.rd for the Administrator’s password.
5. Verify that when the installation is complete, the Command Prompt window on LON-SVR6 opens with a C:\Users\Administrator> prompt. 

>**Results:** After completing this exercise, you will have successfully installed the Windows Server 2016 Core operating system on LON-SVR6. 

## Exercise 2: Completing post-installation tasks on Windows Server 2016 Core

**Scenario**
You must now complete the installation of Server Core by configuring the post-installation settings and joining it to the Adatum.com domain. You will also install the DNS Server role. The main task for this exercise is as follows:

1. Use Windows PowerShell and Sconfig.cmd to configure the settings of Server Core.

**Task 1: Use Windows PowerShell and Sconfig.cmd to configure the settings of  Server Core**
2. Open **Windows PowerShell** on **LON-SVR6**.
3. Use the **$env:computername** and **Get-NetIPAddress** cmdlets to display the **LON-SVR6** host name and IPv4 address information.
4. Note that the name is random and that the address is automatically derived from a DHCP Server.
5. Run the **Sconfig.cmd** tool on **LON-SVR6**. Use the tool to set the following:
   1. IP Address settings: 
      1. Address: **172.16.0.26**
      2. Subnet Mask: **255.255.0.0**
      3. Default Gateway: **172.16.0.1**
      4. Preferred DNS Server: **172.16.0.10**
   2. Join the **Adatum.com** domain and use **Adatum\administrator** credentials.
   3. Rename the computer **LON-SVR6** and use **Adatum\Administrator** credentials.
   4. Restart the computer.
   5. After **LON-SVR6** starts, sign in as **Administrator** with the password **Pa55w.rd**.
6. Start **Windows PowerShell**, and then use the **$env:computername** and **Get-NetIPAddress** cmdlets to display the **LON-SVR6** host name and IPv4 address information. 
7. Note that the name is **LON-SVR6** and that the address is **172.16.0.26**.
8. Type the following, and then press Enter: 
```
Install-WindowsFeature DNS
```
>**Results:** After completing this exercise, you will have successfully configured the domain and network settings of Server Core and install an additional role. 

## Exercise 3: Performing remote management

**Scenario**
Now that you added the DNS Server role to Server Core on **LON-SVR6**, you want to explore configuring the DNS settings and configuration by using GUI tools on other Windows Server with Desktop Experience systems. The main tasks for this exercise are as follows: 
1. Enable remote management with Server Manager. 
2. Add LON-SVR6 to DNS Manager on LON-DC1 and then add the Adatum.com zone to LON-SVR6 as a secondary zone.
3. Examine the new zone information on LON-SVR6.

**Task 1: Enable remote management with Server Manager**
1. On **LON-DC1**, in Server Manager, add **LON-SVR6** to the **Computer** list.
2. Open **DNS Manager**, and then add **LON-SVR6** as a name server in the Adatum.com zone.
3. In DNS Manager, allow zone transfers to all name servers, and then set **Notify** to **172.16.0.26**.

**Task 2: Add LON-SVR6 to DNS Manager on LON-DC1 and then add the Adatum.com zone to LON-SVR6 as a secondary zone**
1. In DNS Manager, on **LON-DC1**, add **LON-SVR6** as an additional DNS server by using the **Connect to DNS Server** window.
2. In DNS Manager, add the **Adatum.com** zone as a secondary zone on **LON-SVR6**, using LON-DC1 as the master DNS server.
3. Refresh **DNS Manager**, and then verify that **LON-SVR6** has the **Adatum.com** zone information and resource records from **LON-DC1**.

**Task 3: Examine the new zone information on LON-SVR6**
1. Return to **LON-SVR6**.
2. Use the **dnscmd** command to enumerate the zones and note the type of zone for Adatum.com: 
```
Dnscmd /enumzones
```
3. Use **dnscmd** to enumerate the zones on LON-DC1 and note the type of zone for Adatum.com: 
```
Dnscmd LON-DC1 /enumzones 
```
4. Display the DNS client server address by using Windows PowerShell: 
```
Get-DnsClientServerAddress
```
5. Set the LON-SVR6 DNS server address, replacing the X with the actual interface index number from step 4:
```
Set-DnsClientServerAddress -InterfaceIndex X -ServerAddresses (“172.16.0.26”, “172.16.0.10”)
```
6. Verify the results. Use the Windows PowerShell command from step 4.

>**Results:** After completing this exercise, you will have configured the DNS Server settings on LON-SVR6remotely. 

**Task 4: Prepare for the next module**

After you finish the lab, revert the virtual machines to their initial state by completing the following steps: 
1. On the host computer, switch to the **Hyper-V Manager** console.
2. In the **Virtual Machines** list, right-click **20740C-LON-DC1**, and then click **Revert**.
3. In the **Revert Virtual Machine** dialog box, click **Revert**.
4. Repeat steps 2 and 3 for **20740C-LON-SVR6**. 