# Lab: Installing and configuring Nano Server 

**Scenario** 
You are responsible for implementing many of the new features in Windows Server 2016. To become familiar with the new operating system, you decide to install a new server running Windows Server 2016 and complete the post-installation configuration tasks. 

**Objectives** 
After completing this lab, you will be able to:
* Install the Nano Server option for Windows Server 2016.
* Configure Nano Server. 

**Lab Setup** 
*Estimated Time:* 60 minutes
*Virtual machines:* **20740B-LON-DC1**, **20740B-NANO-SVR1**
*User name:* **Adatum\Administrator**
*Password:* **Pa55w.rd**

For this lab, you will use the available virtual machine environment. Before you begin the lab, complete the following steps:
1. On the host computer, start **Hyper-V Manager**.
2. In **Hyper-V Manager**, click **20740B-LON-DC1**, and then, in the **Actions** pane, click **Start**.
3. In the **Actions** pane, click **Connect**. Wait until the virtual machine starts.
4. Sign in by using the following credentials:
   * User name: **Adatum\Administrator**
   * Password: **Pa55w.rd**
5. Repeat steps 2 and 3 for **20740B-NANO-SVR1**.
6. On **20740B-LON-DC1**, in the virtual machine connection window, click **Media**, point to **DVD Drive**, and then click **Insert Disk**.
7. Browse to **D:\Program Files\Microsoft Learning\20740\Drives** and then select **WinServer2016_1607.iso**. (or C:\\)
8. Click **Open**. 

## Exercise 1: Installing Nano Server 

**Scenario** 
You determine that Nano Server offers you the best installation option and decide to deploy a web server that uses Nano Server.

The main tasks for this exercise are as follows:
1. Copy the required Windows PowerShell scripts.
2. Import Windows PowerShell modules.  
3. Create a virtual hard drive.
4. Sign in to the NANO-SVR1 virtual machine.

**Task 1: Copy the required Windows PowerShell scripts** 
1. On **LON-DC1**, open an elevated **Windows PowerShell** prompt.
2. Change to the root directory of drive **C**, and then make a folder called **Nano**.
3. Copy all the files with a .ps* extension from the **D:\NanoServer\NanoServerImageGenerator\\** folder to **C:\Nano**. 

**Task 2: Import Windows PowerShell modules**
* Run **Import-Module c:\nano\NanoServerImageGenerator.psm1**. This command imports the required Windows PowerShell module for Nano Server. 

**Task 3: Create a virtual hard drive**
1. Run **new-NanoServerImage -Edition Standard -mediapath D:\ -Basepath c:\nano -targetpath c:\nano\nano-svr1.vhdx -DeploymentType Guest -computername NANO-SVR1 -storage  -package Microsoft-NanoServer-IIS-Package**, and when you receive a prompt, type the password **Pa55w.rd**. 
2. Verify that **C:\Nano** contains a file called **nano-svr1.vhdx**.
 
> **Note:** Normally, you would now create a virtual machine to use the **nano-svr1.vhdx** file. However, to expedite the process, you will start a virtual machine that has already been created.

**Task 4: Sign in to the NANO-SVR1 virtual machine**
* On NANO-SVR1, sign in by using the following credentials:
  * User name: Administrator
  * Password: **Pa55w.rd** 
 
**Results:** After completing this exercise, you will have successfully created the required virtual hard drive for Nano Server. 

## Exercise 2: Completing post-installation tasks on Nano Server
Scenario You must now complete the installation of Nano Server by configuring the post-installation settings and joining it to the Adatum.com domain.

The main tasks for this exercise are as follows:
1. Use the Nano Server Recovery Console to view basic settings.
2. Add Nano Server to the domain. 
3. Use Windows PowerShell to configure the settings of Nano Server. 

**Task 1: Use the Nano Server Recovery Console to view basic settings** 
1. On **NANO-SVR1**, observe that the computer name is **NANO-SVR1** and that the computer is in a workgroup.
2. In **Network Adapter Settings**, notice that DHCP is providing the IP configuration. Make a note of the IP address: _____________ 

**Task 2: Add Nano Server to the domain**
1. On **LON-DC1**, in the **Administrator: Windows PowerShell** window, run **djoin.exe /provision /domain adatum /machine nano-svr1 /savefile .\odjblob.txt**. This creates a file that you will use to complete the process of adding Nano Server to the domain.

> **Note:** Replace the IP address **172.16.0.X** in the following commands with the IP address you recorded earlier from your Nano Server installation. 

2. The following commands are used to enable Windows PowerShell remoting:
    a. **Set-Item WSMan:\localhost\Client\TrustedHosts "172.16.0.X"**
    b. **$ip = "172.16.0.X"**
    c. **Enter-PSSession -ComputerName \$ip -Credential \$ip\\Administrator**
3. In the **Windows PowerShell** credential request dialog box, in the **Password** box, type **Pa55w.rd**, and then click **OK**.
4. To enable file sharing through the firewall, run **netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes**.
5. Close the Windows PowerShell remoting session by running **Exit-PSSession**.
6. Map a network drive to the **C** drive on Nano Server. (net use z: \\172.16.0.X\c$)
7. Switch to the **Z** drive and then copy **C:odjblob** to the root of the **C** drive on Nano Server.
8. Reestablish a Windows PowerShell remoting session to Nano Server.
9. Run **djoin /requestodj /loadfile c:\odjblob /windowspath c:\windows /localos** to complete the process of adding the computer to the domain.
10. Run **shutdown /r /t 5** to restart Nano Server.
11. On **NANO-SVR1**, in the domain **Adatum**, sign in as **Administrator/Pa55w.rd**.
12. In the **Nano Server Recovery Console**, observe that the computer is in the adatum.com domain.

**Task 3: Use Windows PowerShell to configure the settings of Nano Server** 
1. On **LON-DC1**, close **Windows PowerShell**. 
2. Open an elevated **Windows PowerShell** prompt.
3. Run **get-windowsfeature –comp Nano-svr1** to list the installed roles and features on Nano Server.
4. To add the File Server role to Nano Server, run **install-windowsfeature Fs-fileserver –comp Nanosvr1**
5. To verify the role is installed, run **get-windowsfeature –comp Nano-svr1**.  
6. Enable a Windows PowerShell remoting session with Nano Server. Remember to change **X** to the last octet of the IP address of your Nano server:
   a. Run **$ip = "172.16.0.X"**.
   b. Run **Enter-PSSession -ComputerName $ip -Credential $ip\Administrator**.
7. When you receive a prompt, type **Pa55w.rd** as the password.
8. To view the IP configuration of Nano Server, run **get-netipaddress**.
9. To view the startup environment of Nano Server, run **bcdedit /enum**.
10. To view the shared folders, run **net share**. Only default shares exist.
11. At the command prompt, type the following cmdlet, and then press Enter:  **Exit-PSSession**

**Results:** After completing this exercise, you will have successfully configured the domain and network settings of Nano Server and installed an additional role.

## Exercise 3: Performing remote management

**Scenario**
You must add some additional server roles to the Nano server before it can be used in production. You will add and enable the file and web server roles by using remote management.

The main tasks for this exercise are as follows: 

1. Enable remote management with Server Manager.
2. Test the file server and web server on Nano Server.
3. Prepare for the next module.

**Task 1: Enable remote management with Server Manager**
1. On **LON-DC1**, in **Server Manager**, add Nano-SVR1 to the **Computer** list.
2. In Server Manager, expand **File and Storage Services**, click **Shares**, and then in the **TASKS** list, click **New Share**.
3. Create a new shared folder:
    a. Type: **SMB Share - Quick**
    b. Server: **nano-svr1**
    c. Share name: **Data**
    
**Task 2: Test the file server and web server on Nano Server**
1. If necessary, on **LON-DC1**, map drive **Z** to **\\\\Nano-svr1\c$**.
2. Start **Notepad**, and then create a file with the following line.
><H1> Nano Server Website </H1>
3. Save the file called **Default.htm** to **z:\Inetpub\wwwroot**.  
4. Open **Windows Internet Explorer**, and then navigate to **http://nano-svr1**. Does your web page display?
5. Map drive **Y** to **\\Nano-svr1\data**. 
6. Open **WordPad**, create a file, and then save the file to the root of drive **Y**.
7. Use File Explorer to verify that your file is saved on **Nano-Svr1**.

**Task 3: Prepare for the next module**

After you finish the lab, revert the virtual machines to their initial state by completing the following steps: 
1. On the host computer, switch to the Hyper-V Manager console.
2. In the Virtual Machines list, right-click 20740B-LON-DC1, and then click Revert.
3. In the Revert Virtual Machine dialog box, click Revert.
4. Repeat steps 2 and 3 for 20740B-NANO-SVR1. 
 
>**Results:** After completing this exercise, you will have installed and enabled the file and web server roles using remote management. 