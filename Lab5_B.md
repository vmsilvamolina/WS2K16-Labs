# Lab B: Administering AD DS

**Scenario** 
You have been working for the A. Datum Corporation as a desktop support specialist and have performed troubleshooting tasks on desktop computers to resolve application and network problems. You recently accepted a promotion to the server support team. One of your first assignments is to configure the infrastructure service for a new branch office. To begin the deployment of the new branch office, you are preparing AD DS objects. As part of this preparation, you need to create an OU for the branch office and delegate permission to manage it. Also, you need to evaluate Windows PowerShell to manage AD DS more efficiently.
**Objectives**
After completing this lab, you will have:
- Delegated administration for OUs.
- Used Windows PowerShell to manage AD DS objects. 

**Lab Setup**
- Estimated Time: 45 minutes
- Virtual machines: 20742B-LON-DC1, 20742B-LON-SVR1, and 20742B-LON-CL1
- User name: Adatum\Administrator
- Password: Pa55w.rd 

For this lab, you need to use the available VM environment. Before you begin the lab, you must complete the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. In Hyper-V Manager, click 20742B-LON-DC1, and then in the Actions pane, click Start. 
3. In the Actions pane, click Connect. Wait until the VM starts.  
4. Sign in by using the following credentials:  
   1. User name: Administrator
   2. Password: Pa55w.rd 
   3. Domain: Adatum 
5. Repeat steps 1 through 4 for 20742B-LON-SVR1. 
6. Repeat steps 1 through 3 for 20742B-LON-CL1. Do not sign in to LON-CL1 until instructed to do so in the lab steps. 

## Exercise 1: Delegating administration for OUs

**Scenario**
A. Datum Corporation delegates management of each branch office to a specific group. This allows IT employees who work onsite to be administrators for the branch. Each branch office has a branch administrators group that can perform full administration within the branch office OU. There is also a branch office helpdesk group that can manage users in the branch office OU, but not other objects. You need to create the OU and these groups for the new branch office and delegate permissions to the groups. You will also validate the permissions.
The main tasks for this exercise are as follows: 
1. Create a new OU for the branch office. 
2. Create groups for branch administrators and branch help-desk personnel. 
3. Add members to the group. 
4. Delegate permissions to the group. 
5. Test permissions. 

**Task 1: Create a new OU for the branch office**
- On LON-DC1, use Active Directory Users and Computers to create a new OU named London. 

**Task 2: Create groups for branch administrators and branch help-desk personnel** 
- In the London OU, create the following global security groups: 
  - London Admins 
  - London Helpdesk 
  
**Task 3: Add members to the group**
1. Select the IT OU. 
2. Add Beth Burke to the London Admins group. 
3. Add Dante Dabney to the London Helpdesk group. 

**Task 4: Delegate permissions to the group**
1. In Active Directory Users and Computers, enable the Advanced Features view.
2. Set permissions on the Security tab of the London OU properties to give the London Admins group Full Control. 
3. Use the Delegation of Control Wizard to grant Full Control over User objects in the London OU to the London Helpdesk group.

**Task 5: Test permissions**
- On LON-SVR1, use the Add Roles and Features Wizard in Server Manager to install the AD DS Tools feature. When this task is complete, sign out of LON-SVR1. 
Test permissions for London Admins 
1. Sign in to LON-SVR1 as Beth with the password Pa55w.rd.
2. Open Active Directory Users and Computers. 
3. Click the Research OU. Notice that the icons on the toolbar for creating users, groups, or OUs are dimmed. 
4. Click the London OU, and then notice that those icons are available now. 
5. Create a sub-OU in the London OU named Laptops. 
6. Sign out of LON-SVR1. 

Test permissions for London Helpdesk
1. Sign in to LON-SVR1 as Dante with the password Pa55w.rd. 
2. Open Active Directory Users and Computers. 
3. Select the London OU. Notice that the only available icon is the create user icon. 
 
>**Results:** After completing this exercise, you will have: 
> - Created a new OU for the branch office.
> - Created groups for branch administrators and branch help-desk personnel. 
> - Added members to the group. 
> - Delegated permission to the groups. 
> - Installed Active Directory Domain Services (AD DS) tools and tested permissions. 

## Exercise 2: Creating and modifying AD DS objects with Windows PowerShell

**Scenario**
A. Datum Corporation has several scripts that administrators have used in the past to create user accounts by using command-line tools. However, an enterprise-wide mandate specifies that administrators will use Windows PowerShell to perform all future scripting. As the first step in creating scripts, you need to identify the syntax required to manage AD DS objects in Windows PowerShell.  You have a .csv file that contains a large list of new users for the branch office. It is inefficient to create these users individually with graphical tools, so you will use a Windows PowerShell script instead. A colleague who has experience with scripting has given you a script that she created. You need to sign in as branch administrator and modify the script to match the format of your .csv file. 
The main tasks for this exercise are as follows: 
1. Create a user account by using Windows PowerShell.
2. Create a new group by using Windows PowerShell. 
3. Add a member to the group by using Windows PowerShell. 
4. Modify the .csv file. 
5. Modify the script. 
6. Run the script. 
7. Prepare for the next module. 

**Task 1: Create a user account by using Windows PowerShell**
1. Switch to LON-DC1. 
2. Start an elevated session of Windows PowerShell. 
3. Create a user account for Ty Carlson in the London OU by running the following command: 
`New‐ADUser ‐Name Ty ‐DisplayName "Ty Carlson" ‐GivenName Ty ‐Surname Carlson ‐Path "ou=London,dc=adatum,dc=com"` 
4. Run the following command to set the password as Pa55w.rd: 
`Set-ADAccountPassword Ty` 
The current password is blank. 
5. Run the following command to enable the account: 
`Enable-ADAccount Ty`
1. Test the account by switching to LON-CL1, and then sign in as Ty with the password Pa55w.rd.

**Task 2: Create a new group by using Windows PowerShell**
- On LON-DC1, in the Administrator: Windows PowerShell window, run the following command: `New‐ADGroup LondonBranchUsers ‐Path "ou=London,dc=adatum,dc=com" ‐GroupScope Global ‐GroupCategory Security` 

**Task 3: Add a member to the group by using Windows PowerShell** 
1. In the Administrator: Windows PowerShell window, run the following command: `Add‐ADGroupMember LondonBranchUsers ‐Members Ty` 
2. Confirm that the user is in the group by running the following command: `Get‐ADGroupMember LondonBranchUsers` 

**Task 4: Modify the .csv file**
1. Use Notepad to modify the .csv file by adding the following header: 
`FirstName,LastName,Department,DefaultPassword `
1. Save and close the file. 

**Task 5: Modify the script** 
1. In the Administrator: Windows PowerShell (ISE) window, replace these variables: 
`C:\path\file.csv with E:\Labfiles\Mod02\LabUsers.csv "ou=orgunit,dc=domain,dc=com" with "ou=London,dc=adatum,dc=com"`
2. Save the file, and then close the Administrator: Windows PowerShell (ISE) window. 

**Task 6: Run the script**
1. Switch to the Administrator: Windows PowerShell window.
2. Change the root directory to E:\Labfiles\Mod02.
3. Type .\LabUsers.ps1 to run the script. 
4. Run the following command to ensure that you created the users: `Get‐ADUser ‐Filter * ‐SearchBase "ou=London,dc=adatum,dc=com"` 

**Task 7: Prepare for the next module**
When you are finished with the lab, revert all VMs to their initial state: 
1. On the host computer, start Hyper-V Manager. 
2. In the Virtual Machines list, right-click 20742B-LON-DC1, and then click Revert. 
3. In the Revert Virtual Machine dialog box, click Revert. 
4. Repeat steps 2 and 3 for 20742B-LON-SVR1 and 20742B-LON-CL1. 
 
>**Results:** After completing this lab, you will have: 
> - Created a user account by using Windows PowerShell.
> - Created a group by using Windows PowerShell.
> - Added a user to a group by using Windows PowerShell.
> - Modified the .csv file.
> - Modified the script.
> - Run the script.