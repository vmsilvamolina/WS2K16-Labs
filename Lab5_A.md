# Lab A: Managing AD DS objects

**Scenario** You have been working for A. Datum Corporation as a desktop support specialist and have visited desktop computers to troubleshoot app and network problems. You recently accepted a promotion to the server support team. One of your first assignments is to configure the infrastructure service for a new branch office. To begin deployment of the new branch office, you are preparing AD DS objects. As part of this preparation, you need to create users and groups for the new branch office that will house the Research department. Finally, you need to reset the secure channel for a computer account that has lost connectivity to the domain in the branch office.

**Objectives**
After completing this lab, you will have:
- Created and configured groups in AD DS.
- Created and configured user accounts in AD DS.
- Managed computer objects in AD DS.

**Lab Setup**
- Estimated Time: 45 minutes
- Virtual machines: 20742B-LON-DC1 and 20742B-LON-CL1
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
5. Repeat steps 1 through 3 for 20742B-LON-CL1. Do not sign in to LON-CL1 until instructed to do so in the lab steps. 

## Exercise 1: Creating and managing groups in AD DS

**Scenario** You need to create groups for the Research department. You require a distribution global group to facilitate email delivery to the Research users. Cai Chu will manage this group. You will also create a Research Managers group and add Cai Chu and Vera Pace as members of the group. You also need to create a universal group in the Managers OU that will contain all the departmental managers’ global groups. After creating the research distribution group, you realize that the group needs access to network resources, so you must convert the group to a security group. 
The main tasks for this exercise are as follows:
1. Create groups and add members.
2. Configure group nesting.
3. Convert a group type from distribution to security.

**Task 1: Create groups and add members**
1. On LON-DC1, use Active Directory Administrative Center to create the following groups:
   1. In the Managers OU, create a universal group named Enterprise Managers. 
   2. In the Research OU, create a global distribution group named Research Mail. 
2. Configure the email address of the Research Mail group to be Research@adatum.com. 
3. Configure Cai Chu as the manager of the Research Mail group. 
4. Provide Cai Chu with the right to update the group membership list. 
5. In the Research OU, create a new global security group named Research Managers. 
6. Add Cai Chu and Vera Pace as members.  

**Task 2: Configure group nesting** 
- Browse to the Managers OU, and then add the Managers global group and the Research Managers global group as members of the Enterprise Managers universal group. 

**Task 3: Convert a group type from distribution to security** 
- In the Research OU, change the group type of the Research Mail group to be a Security group. 
 
>**Results:** After completing this exercise, you will have: 
> - Created groups and added members. 
> - Configured group nesting. 
> - Converted a group type. 

# Exercise 2: Creating and configuring user accounts in AD DS

**Scenario** You have a list of new users that you must create for the branch office. You have decided to create a template to facilitate the quick creation of users for the branch. You will validate that template by creating a new test user and checking the properties. 
The main tasks for this exercise are as follows: 
1. Create and configure a user template for the Research department. 
2. Create new users for the Research branch office based on the template. 
3. Validate the template. 

**Task 1: Create and configure a user template for the Research department** 
1. Create a new user in the Research OU with the following properties:
   - Name: _Research Template
   - User UPN Logon: ResearchTemplate 
   - Password: Pa55w.rd 
   - Department: Research
   - Company: Adatum
   - Manager: Cai Chu 
   - Member of: Research 
   - Logon script: \\\\LON-DC1\\Netlogon\\Logon.bat 
 2. Disable the account. 
 3. Close Active Directory Administrative Center. 
 
 **Task 2: Create new users for the Research branch office based on the template** 
 1. Use Active Directory Users and Computers to copy the _Research Template account. 
 2. Create the new user from the template with the following properties: 
    - First name: Research 
    - Last name: User 
    - Password: Pa55w.rd 
    - Account status: Enabled 
    
**Task 3: Validate the template**
- Inspect the properties of the Research User and ensure that the properties are: 
  - Logon script: \\\\LON-DC1\\Netlogon\\Logon.bat 
  - Department: Research 
  - Company: Adatum 
  - Member Of: Research 
 
>**Results:** After completing this exercise, you will have:
> - Created and configured a user template for Research users.
> - Created three new users based on the template. 
> - Signed in to test that the accounts are functioning as expected.

# Exercise 3: Managing computer objects in AD DS 

**Scenario**
A user is unable to sign in, and a workstation has lost its connectivity to the domain and cannot authenticate users properly. When users attempt to access resources from this workstation, access is denied. You need to repair the trust relationship between the computer and the domain. 
The main tasks for this exercise are as follows: 
1. Reset a computer account. 
2. Observe the behavior when a client attempts to sign in. 
3. Resolve the computer issue. 

**Task 1: Reset a computer account**
- Right-click LON-CL1 in the computers container and reset the account.

**Task 2: Observe the behavior when a client attempts to sign in**
- Restart LON-CL1, and then attempt to sign in as Adatum\Adam with the password Pa55w.rd.  

> **Question:** What is the message displayed? Answer: The trust relationship between this workstation and the primary domain failed. 

**Task 3: Resolve the computer issue** 
1. Sign in to LON-CL1 as Adatum\Administrator with the password Pa55w.rd. 
2. Start an elevated session of Windows PowerShell, and then run the following command: 
`Test-ComputerSecureChannel –Repair`
3. Sign out of LON-CL1, and then attempt to sign in as Adatum\Adam. The sign in will succeed now. 
4. Sign out of LON-CL1. 
5. Leave the VMs running for the next lab. 

 >**Results:** After completing this exercise, you will have:
> - Reset a computer account.
> - Observed the behavior when a client signs in.
> - Resolved the computer issue. 