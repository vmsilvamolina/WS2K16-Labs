# Lab: Deploying and administering AD DS

**Scenario**
You are an IT administrator at A. Datum Corporation. The company is expanding its business and has several new locations. The AD DS administration team is currently evaluating the methods available in Windows Server 2016 for a rapid and remote domain controller deployment. Also, the team is looking for a way to automate certain AD DS administrative tasks. The team wants a fast and seamless deployment of new domain controllers for new locations, and it wants to promote servers to domain controllers from a central location.

**Objectives**
After completing this lab, you will have:
- Deployed AD DS.
- Deployed domain controllers by performing domain controller cloning.
- Administered AD DS. 

**Lab Setup**
- Estimated Time: 45 minutes
- Virtual machines: 20742B-LON-DC1 and 20742B-LON-SVR1
- User name: Adatum\Administrator
- Password: Pa55w.rd

For this lab, you need to use the available virtual machine environment. Before you begin the lab, you must complete the following steps:
1. On the host computer, start Hyper-V Manager.
2. In Hyper-V Manager, click 20742B-LON-DC1, and then in the Actions pane, click Start.
3. In the Actions pane, click Connect. Wait until the virtual machine starts.
4. Sign in by using the following credentials:
    1. User name: Adatum\Administrator
    2. Password: Pa55w.rd
5. Repeat steps 2 through 4 for virtual machine 20742B-LON-SVR1.

## Exercise 1: Deploying AD DS

**Scenario**
As a part of the business expansion, A. Datum Corporation wants to deploy new domain controllers in remote sites with minimal engagement from the remote IT staff. You will use Windows PowerShell to deploy new domain controllers. 
The main tasks for this exercise are as follows:
1. Install AD DS binaries.
2. Prepare the AD DS installation and promote a remote server.
3. Run the AD DS Best Practices Analyzer. 

**Task 1: Install AD DS binaries**
1. Switch to LON-DC1.
2. From Server Manager, open Windows PowerShell.
3. Use the Install-WindowsFeature cmdlet in Windows PowerShell to install the AD DS role on  LON-SVR1.
4. Use the Get-WindowsFeature cmdlet to verify the installation.
5. Ensure that the check boxes for Active Directory Domain Services, Remote Server Administration Tools, and Role Administration Tools are selected. For the AD DS and AD LDS Tools node, only the Active Directory module for Windows PowerShell should be installed, and not the graphical tools, such as the Active Directory Administrative Center.  

>*Note:* If you centrally manage your servers, you will not usually need GUI tools on each server. If you want to install them, you need to specify the AD DS tools by running the AddWindowsFeature cmdlet with the RSAT-ADDS command name.  
    
>*Note:* You might need to wait a short time after the installation process completes before verifying that the AD DS role has installed. If you do not see the expected results from the Get-WindowsFeature command, you can try again after a few minutes. 

**Task 2: Prepare the AD DS installation and promote a remote server**
Add LON-SVR1 to Server Manager on LON-DC1 
- On LON-DC1, from Server Manager, on the All Servers node, add LON-SVR1 as a managed server. 
Remotely configure AD DS by using Server Manager
1. On LON-DC1, from Server Manager, configure LON-SVR1 as an AD DS domain controller by using the following settings:
   1. Type: Additional domain controller for existing domain
   2. Domain: Adatum.com
   3. Credentials: Adatum\Administrator with the password Pa55w.rd
   4. Directory Services Restore Mode (DSRM) Password: Pa55w.rd
   5. Remove the selections for DNS and the global catalog. 
2. On the Review Options page, click View Script.
3. In Notepad, edit the generated Windows PowerShell script:
   1. Delete the comment lines, which begin with the number sign (#).
   2. Remove the Import-Module line.
   3. Remove the grave accents (`) at the end of each line. 
   4. Remove the line breaks. 
4. Now that the Install-ADDSDomainController command and all the parameters are on one line, copy the command. 
5. Switch to the Active Directory Domain Services Configuration Wizard, and then click Cancel. 
6. Start Windows PowerShell, and then at the command prompt, type the following command: 
`Invoke-Command –ComputerName LON-SVR1 { }`
7. Paste the copied command between the braces ({ }), and then press Enter to start the installation.
8. Provide the following credentials: 
   1. User name: Adatum\Administrator
   2. Password: Pa55w.rd 
9. Type and confirm the SafeModeAdministratorPassword as Pa55w.rd. 
10. After LON-SVR1 restarts, on LON-DC1, switch to Server Manager, and then on the left side, click the AD DS node. Note that LON-SVR1 has been added as a server and that the warning notification has disappeared. You might have to click Refresh. 

**Task 3: Run the AD DS Best Practices Analyzer**
1. On LON-DC1, in Server Manager, go to the AD DS dashboard view. 
2. Start the Best Practices Analyzer (BPA) scan for LON-DC1 and LON-SVR1. 
3. Review the results of the BPA. 
 
>**Results:** After this exercise, you should have successfully created a new domain controller and reviewed the Active Directory Domain Services (AD DS) Best Practices Analyzer (BPA) results for that domain controller. 

## Exercise 2: Deploying domain controllers by performing domain controller cloning

**Scenario**
An IT team at A. Datum Corporation wants to rapidly deploy new virtual domain controllers as needed. They are considering the domain controller clones in Windows Server 2016. You must perform a domain controller cloning procedure to verify that it is a valid option to speed up the deployment of domain controllers. 

>*Note:* You might not be able to perform this exercise in a cloud-based lab environment. This exercise requires you to have local access to the Hyper-V host. 

The main tasks for this exercise are as follows: 
1. Check for domain controller clone prerequisites. 
2. Copy the source domain controller. 
3. Perform domain controller cloning. 

**Task 1: Check for domain controller clone prerequisites** 
1. Switch to LON-DC1. 
2. In Server Manager, open the Active Directory Administrative Center, and then add LON-DC1 to the Cloneable Domain Controllers group. 
3. Open Windows PowerShell and use Get-ADDCCloningExcludedApplicationList to verify that the apps and services on LON-DC1 support cloning. 
4. Use the Get-ADDCCloningExcludedApplicationList –GenerateXML and  New-ADDCCloneConfigFile cmdlets to create a DCCloneConfig.xml file. 

**Task 2: Copy the source domain controller** 
1. Shut down LON-DC1. 
2. On the host computer, in Hyper-V Manager, export LON-DC1 to D:\Program Files \Microsoft Learning\20742\.

>*Note:* Depending on your classroom’s setup, the Program Files\Microsoft Learning \20742 folder might be on drive C. Please locate and use the existing folder for the remainder of the lab.

1. Start LON-DC1, and then sign in as Adatum\Administrator with the password Pa55w.rd.

**Task 3: Perform domain controller cloning** 
1. On the host computer, start the Import Virtual Machine Wizard in Hyper-V Manager. 
2. On the Locate Folder page, go to the D:\Program Files\Microsoft Learning\20742 \20742B-LON-DC1 folder. 
3. On the Select Virtual Machine page, select 20742B-LON-DC1. 
4. On the Choose Import Type page, select Copy the virtual machine (create a new unique ID). 
5. On the Choose Folders for Virtual Machine Files page, select the Store the virtual machine in a different location check box. For each folder location, provide the path D:\Program Files \Microsoft Learning\20742\. 
6. On the Choose Folders to Store Virtual Hard Disks page, provide the path D:\Program Files \Microsoft Learning\20742\.  
7. Rename the new virtual machine 20742B-LON-DC3. 
8. In Hyper-V Manager, start and connect to 20742B-LON-DC3. 
9. While the server is starting, you might see the message “Domain Controller cloning is at x% completion.” 
 
>**Results:** After completing this exercise, you should have successfully deployed a domain controller by cloning it in Microsoft Hyper-V.  

## Exercise 3: Administering AD DS

**Scenario**
The IT team at A. Datum Corporation is evaluating the tools that are available in Windows Server 2016 for AD DS administration. You should evaluate the use of both the Active Directory Administrative Center and Windows PowerShell for AD DS administration and management. 

The main task for this exercise is as follows: 
1. Use the Active Directory Administrative Center. 

**Task 1: Use the Active Directory Administrative Center**
Navigate within the Active Directory Administrative Center 
1. On LON-DC1, from Server Manager, open the Active Directory Administrative Center. 
2. Switch to the tree view, and then expand Adatum (local). 

Perform an administrative task within the Active Directory Administrative Center 
1. Go to the Overview view. 
2. Reset the password for Adatum\Adam to Pa55w.rd without requiring the user to change the password at the next sign-in. 
3. Use the Global Search section to find any objects that match the “Lon” search string. 

Create objects
- Create a new computer object named LON-CL4 in the Computers container. 
View all object attributes
- Open the Properties page for LON-CL4, scroll to the Extensions section, and then select the Attribute Editor tab. View the objects’ AD DS attributes. 
Use the Windows PowerShell History viewer 
1. Open the Windows PowerShell History pane. 
2. View the Windows PowerShell cmdlet that you used to perform the most recent task. 
 
>**Results:** After completing this exercise, you should have successfully used the Active Directory Administrative Center to manage AD DS and reviewed the Windows PowerShell cmdlets that run in the background. 

**Task 2: Prepare for the next module**
When you are finished with the lab, revert all virtual machines to their initial state: 
1. On the host computer, start Hyper-V Manager. 
2. In the Virtual Machines list, right-click 20742B-LON-DC1, and then click Revert. 
3. In the Revert Virtual Machine dialog box, click Revert. 
4. Repeat steps 2 and 3 for 20742B-LON-SVR1. 