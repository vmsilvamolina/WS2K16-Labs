## Lab A: Implementing a Group Policy infrastructure

**Scenario**
Your manager asked you to use Group Policy to implement standardized security settings to lock computer screens when users leave computers unattended for 10 minutes or more. You also have to configure a policy setting that will prevent access to certain programs on local computers. You configured Group Policy to lock computer screens when users leave computers unattended for 10 minutes or more. However, after some time, you were made aware that a critical application used by the Research engineering team fails when the screen saver starts. An engineer asked you to prevent the GPO setting from applying to any member of the Research security group. He also asked you to configure conference room computers to be exempt from corporate policy. However, you must ensure that the conference room computers use a 2-hour time out. Create the policies that you need to evaluate the RSoPs for users in your environment. Make sure to optimize the Group Policy infrastructure and verify that all policies are applied as they were intended.

**Objectives** 
After completing this lab, you will be able to:
- Create and configure GPOs.
- Manage GPO scope. 
 
**Lab Setup**
- Estimated Time: 40 minutes
- Virtual machines: 20742B-LON-DC1, 20742B-LON-CL1
- User name: Adatum\Administrator
- Password: Pa55w.rd 

For this lab, you need to use the available virtual machine environment. Before you begin the lab, you must complete the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. In Hyper-V Manager, click 20742B-LON-DC1, and in the Actions pane, click Start. 
3. In the Actions pane, click Connect. Wait until the virtual machine starts.  
4. Sign in by using the following credentials:
- User name: Administrator
- Password: Pa55w.rd
- Domain: Adatum 
5. Repeat steps 2 and 3 for 20742B-LON-CL1. Do not sign in to LON-CL1 until directed to do so. 

### Exercise 1: Creating and configuring GPOs

**Scenario**
Your manager asked you to use Group Policy to implement standardized security settings to lock computer screens when users leave computers unattended for 10 minutes or more. She also asked you to configure a policy setting that will prevent access to registry-editing tools on local computers. 
The main tasks for this exercise are as follows: 
1. Create and edit a GPO. 
2. Link the GPO. 
3. View the effects of the GPO’s settings. 

**Task 1: Create and edit a GPO** 
1. On LON-DC1, from Server Manager, open Group Policy Management Console.  
2. Create a GPO named ADATUM Standards in the Group Policy Objects container. 
3. Edit the ADATUM Standards policy, and then navigate to User Configuration\Policies \Administrative Templates\System. 
4. Prevent users from accessing the registry by enabling the Prevent access to registry editing tools policy setting. 
5. Navigate to the User Configuration\Policies\Administrative Templates\Control Panel \Personalization folder, and then configure the Screen saver timeout policy to 600 seconds. 
6. Enable the Password protect the screen saver policy setting, and then close the Group Policy Management Editor window.

**Task 2: Link the GPO**
- Link the ADATUM Standards GPO to the Adatum.com domain.

**Task 3: View the effects of the GPO’s settings** 
1. Sign in to LON-CL1 as Adatum\Administrator with the password Pa55w.rd. 
2. Open Control Panel. In Windows Firewall, allow Remote Event Log Management and Windows Management Instrumentation (WMI) traffic. 
3. Sign out and then sign in as Adatum\Connie with the password Pa55w.rd. 4. Attempt to change the screen saver wait time and resume settings. You are prevented from doing this by Group Policy. 5. Attempt to run Registry Editor. You are prevented from doing this by Group Policy. 
 
>**Results:** After completing this exercise, you should have created, edited, and linked the required GPO successfully. 

**Exercise 2: Managing GPO scope**
Scenario
You used Group Policy to implement standardized security settings to lock computer screens when users leave computers unattended for 10 minutes or more. However, after some time, an engineer informed you that a critical application used by the Research engineering team fails when the screen saver starts. He asked you to prevent the GPO setting from applying to any member of the Research security group. He also asked you to configure conference room computers to be exempt from corporate policy. However, you must ensure that the conference room computers always use a 2-hour time out. 
The main tasks for this exercise are as follows: 
1. Create and link the required GPOs. 
2. Verify the order of precedence. 
3. Configure the scope of a GPO with security filtering. 
4. Configure loopback processing. 
5. Prepare for the next lab. 

**Task 1: Create and link the required GPOs**
1. On LON-DC1, in Group Policy Management Console, create a new GPO named Research Application Override that is linked to the Research OU. 
2. Configure the Screen saver timeout policy setting to be disabled, and then close the Group Policy Management Editor window. 

**Task 2: Verify the order of precedence**
- In the Group Policy Management Console tree, select the Research OU, and then click the Group Policy Inheritance tab. Notice that the Research Application Override GPO has precedence over the ADATUM Standards GPO. The screen saver time-out policy setting that you just configured in the Research Application Override GPO will be applied after the setting in the ADATUM Standards GPO. Therefore, the new setting will overwrite the standards setting and will prevail. Screen saver timeout will be unavailable for users within the scope of the Research Application Override GPO. 

**Task 3: Configure the scope of a GPO with security filtering**
1. On LON-DC1, in Group Policy Management Console, select the Research Application Override GPO. Notice that in the Security Filtering section, the GPO applies by default to all authenticated users. 
2. In the Security Filtering section, remove Authenticated Users and add the Research group and LON-CL1. 

**Task 4: Configure loopback processing** 
1. On LON-DC1, in Group Policy Management Console, create a new OU named Kiosks under the domain. 
2. Under Kiosks, create a child OU named Conference Rooms. 
3. Create a new GPO named Conference Room Settings, and then link it to the Conference Rooms OU. 
4. Edit the Conference Room Settings GPO, and then modify the Screen saver timeout policy to launch the screen saver after 120 minutes.  
5. In the Computer Configuration section of the GPO, modify the Configure user Group Policy loopback processing mode policy setting to use Merge mode. 
 
> **Results:** After completing this exercise, you should have configured the required scope of the GPOs successfully. 


**Task 5: Prepare for the next lab**
- After you finish this lab, leave the virtual machines running for the next lab.