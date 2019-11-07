## Lab: Recovering objects in AD DS

**Scenario**
Yesterday, you discovered that one user account was deleted by accident. A few days ago, additional user accounts were deleted accidentally. You want to recover these accounts. It is your responsibility to ensure that the directory service is backed up. Today, you notice that last night's backup did not run as scheduled. You therefore decide to perform an interactive backup. Shortly after the backup, a domain administrator accidentally deletes the IT OU. You must recover this OU. 

**Objectives**
After you complete this lab, you will be able to:
- Backup and restore AD DS.
- Recover objects in AD DS. 

**Lab Setup**
- Estimated Time: 60 minutes
- Virtual machine: 20742B-LON-DC1 
- User name: Adatum\Administrator 
- Password: Pa55w.rd 

For this lab, you need to use the available virtual machine environment. Before you begin the lab, you must complete the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. In Hyper-V Manager, click 20742B-LON-DC1, and then in the Actions pane, click Start. 
3. In the Actions pane, click Connect. Wait until the virtual machine starts.  
4. Sign in by using the following credentials:
- User name: Administrator
- Password: Pa55w.rd
- Domain: Adatum 

### Exercise 1: Backing up and restoring AD DS

**Scenario**
You notice that AD DS has not been backed up recently. You decide to create a backup schedule and perform a one-time interactive backup, to be safe. Shortly after the interactive backup, someone deletes an AD DS object inadvertently. You must restore this object authoritatively. 

The main tasks for this exercise are as follows: 
1. Install the Windows Server Backup feature. 
2. Create a scheduled backup. 3. Perform an interactive backup. 
4. Delete an OU. 
5. Restart in Directory Services Restore Mode (DSRM). 
6. Restore System state data. 
7. Mark restored information as authoritative. 
8. Verify that the data has been restored. 

**Task 1: Install the Windows Server Backup feature**
1. On LON-DC1, from Server Manager, install the Windows Server Backup feature. 
2. Open a command prompt, type the following, and then press Enter: 
`cacls C:\Windows\System32\InputMethod\CHS\chsime.exe /E /P system:R`
    
    Note: This command is only required for the lab environment, and is not part of typical backup procedures. 

**Task 2: Create a scheduled backup**
1. On LON-DC1, run Windows Server Backup. 
2. Create a custom backup configuration by using the following information: 
- Items to back up: Bare metal recovery 
- Backup interval: Once a day 
- Time of day: 12:00 am 
- Destination type: Back up to a hard disk that is dedicated for backups 
- Destination disk: Disk 1 
3. When the Windows Server Backup dialog box appears, informing you that all data on the disk will be deleted, click Yes. 
    
    Note: You will cancel the process in the next step to avoid formatting drive E. 

4. Click Cancel. Do not format drive E. 

**Task 3: Perform an interactive backup** 
1. In the Actions pane, click Backup Once. 
2. Configure the backup to use the following settings: 
- Backup configuration: Custom 
- Backup items: System state 
- Advanced setting: VSS full Backup 

    Note: The backup will take about 10–15 minutes to complete. After the backup completes, close Windows Server Backup. 

**Task 4: Delete an OU**

    Note: Wait until the backup completes before proceeding. 

1. On LON-DC1, from Server Manager, open Active Directory Users and Computers. 
2. Delete the Research OU. 

**Task 5: Restart in Directory Services Restore Mode (DSRM)** 
1. On LON-DC1, run Windows PowerShell as an administrator. 
2. At the command prompt, type the following command to configure the server to start in DSRM:  
`bcdedit /set safeboot dsrepair`
1. Restart LON-DC1. 

**Task 6: Restore System state data**
1. Sign in to LON-DC1 as Administrator with the password Pa55w.rd. 
2. Run Windows PowerShell as an administrator. 
3. To obtain the version identifier for the backup, at the command prompt, type the following command, and then press Enter: 
`wbadmin get versions -backuptarget:E: -machine:LON-DC1`
4. Restore the System state information by typing the following command in the following format: 
`wbadmin start systemstaterecovery -version:<version> -backuptarget:E: -machine:LONDC1`
For example:  
`wbadmin start systemstaterecovery -version:01/22/2011-10:37 -backuptarget:E: machine:LON-DC1`
    
    Note: The restoration will take about 30–35 minutes. 

5. When prompted to restart, type Y, and then press Enter. 

**Task 7: Mark restored information as authoritative**
1. Sign in to LON-DC1 as Administrator with the password Pa55w.rd. 
2. Open Windows PowerShell as an administrator. 
3. At the Windows PowerShell command prompt, use NtdsUtil.exe to perform an authoritative restore of “OU=Research,DC=adatum,DC=com” 
4. To restart the server normally after you perform the restoration operation, type the following command, and then press Enter: 
`bcdedit /deletevalue safeboot`
1. Restart the server.

**Task 8: Verify that the data has been restored** 
1. After the server restarts, sign in to LON-DC1 as Adatum\Administrator with the password Pa55w.rd. 
2. From Server Manager, open Active Directory Users and Computers. 
3. Verify the presence of the Research OU. Note that you might have to force a site replication in Active Directory Sites and Services to see the change immediately. 
 
>**Results:** After completing this exercise, you should have successfully performed an interactive backup and an authoritative restore of Active Directory Domain System (AD DS).

### Exercise 2: Recovering objects in AD DS

**Scenario**
Several user accounts have recently been deleted in error. You decide to enable the Active Directory Recycle Bin feature to help with future account recovery. 
The main tasks for this exercise are as follows: 
1. Verify requirements for Active Directory Recycle Bin. 
2. Enable the Active Directory Recycle Bin feature. 
3. Delete objects to simulate accidental deletion. 
4. Perform object restoration with the Active Directory Module for Windows PowerShell. 
5. Verify object restoration. 
6. Prepare for course completion. 

**Task 1: Verify requirements for Active Directory Recycle Bin**
- On LON-DC1, open Active Directory Domains and Trusts and verify the forest functional level. It should be Windows Server 2012 R2.  

**Task 2: Enable the Active Directory Recycle Bin feature**
1. On LON-DC1, start Active Directory Module for Windows PowerShell.
2. Enable the Active Directory Recycle Bin feature by typing the following command, and then pressing Enter:
`Enable-ADOptionalFeature –Identity ‘CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration, DC=adatum,DC=com’ –Scope ForestOrConfigurationSet –Target ‘adatum.com’`

**Task 3: Delete objects to simulate accidental deletion** 
1. Open Active Directory Users and Computers. Provide the account Adatum\Administrator with the password Pa55w.rd. 
2. Navigate to the Sales OU. 
3. Delete Abbie Parsons. 
4. Close Active Directory Users and Computers. 

**Task 4: Perform object restoration with the Active Directory Module for Windows PowerShell**
1. Start the Active Directory Module for Windows PowerShell. 
2. At the Windows PowerShell command prompt, type the following command, and then press Enter: 
`Get-ADObject -Filter {displayName -eq "Abbie Parsons "} -IncludeDeletedObjects | Restore-ADObject`
1. Close the Windows PowerShell window. 

**Task 5: Verify object restoration**
1. On LON-DC1, open the Active Directory Users and Computers console.  
2. Make sure that Abbie Parsons exists within the Sales OU. 
 
>**Results:** After completing the exercise, you should have enabled and tested the Active Directory Recycle Bin feature successfully.

**Task 6: Prepare for course completion** 
When you finish the lab, revert the virtual machines to their initial state. To do this, perform the following steps: 
1. On the host computer, start Hyper-V Manager.  
2. In the Virtual Machines list, right-click 20742B-LON-DC1, and then click Revert. 
3. In the Revert Virtual Machine dialog box, click Revert. 