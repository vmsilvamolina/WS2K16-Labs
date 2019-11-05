## Lab B: Troubleshooting Group Policy infrastructure
**Scenario**
After configuring settings for the Research department and computers in the conference rooms, you want to make sure that all settings apply as intended. You want to do this by creating RSoP reports from both Group Policy Management Console and a client. You do not have access to a computer in the conference rooms, so you have to simulate how settings will apply by using Group Policy modeling analyses. You want to investigate what events are stored in Event Viewer regarding Group Policy. After some time, you receive a Help desk ticket opened by a user. The issue is that the Screen Saver settings that was applied is not the correct settings for the user. You have to investigate the issue and make sure that the correct settings apply to the user.

**Objectives**
After completing this lab, you will be able to: 
- Verify GPO application.
- Troubleshoot GPOs. 

**Lab Setup**
- Estimated Time: 40 minutes
- Virtual machines: 20742B-LON-DC1, 20742B-LON-CL1 
- User name: Adatum\Administrator 
- Password: Pa55w.rd 

For this lab, you need to use the available virtual machine environment. Before you begin the lab, you must complete the Lab A. Also, ensure that the following virtual machines are running. To start each virtual machine, use the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. In Hyper-V Manager, click 20742B-LON-DC1, and in the Actions pane, click Start. 
3. In the Actions pane, click Connect. Wait until the virtual machine starts.  
4. Sign in by using the following credentials:
- User name: Administrator
- Password: Pa55w.rd
- Domain: Adatum 
5. Repeat steps 2 and 3 for 20742B-LON-CL1. Do not sign in to LON-CL1 until directed to do so. 

### Exercise 1: Verifying GPO application

**Scenario**
After configuring settings for the Research department and computers in the conference rooms, to make sure that all settings apply as intended, you need to create RSoP reports from both Group Policy Management Console and a client. You do not have access to a computer in the conference rooms, so you have to simulate how settings will apply by using Group Policy modeling analyses. You also need to investigate what events are stored in Event Viewer regarding Group Policy.
The main tasks for this exercise are as follows: 
1. Perform RSoP analysis. 
2. Analyze RSoP with GPResult. 
3. Evaluate GPO results by using Group Policy Modeling Wizard. 
4. Review policy events. 

**Task 1: Perform RSoP analysis** 
1. On LON-CL1, verify that you are still signed in as Adatum\Connie. If necessary, use the password Pa55w.rd. 
2. Open a command prompt. 
3. Run the `gpupdate /force` command.
4. After the command has completed, make a note of the current system time, which you will need to know for a task later in this lab: 
> Time
5. Restart LON-CL1, and then wait for it to restart before proceeding with the next task. 
6. On LON-DC1, switch to Group Policy Management Console. 
7. Use Group Policy Results Wizard to run an RSoP report for Connie on LON-CL1. 
8. Review summary results. For both user and computer configuration, identify the time of the last policy refresh and the list of allowed and denied GPOs. Identify the components that were used to process policy settings. 
9. Click the Details tab. Review the settings that were applied during user and computer policy application, and then identify the GPO from which the settings were obtained. 
10. Click the Policy Events tab, and then locate the event that logs the policy refresh that you triggered with the gpupdate command. 
11. Click the Summary tab, right-click the page, and then choose Save Report. Save the report as an HTML file on your desktop. Then open the RSoP report in Internet Explorer from the desktop.

**Task 2: Analyze RSoP with GPResult** 
1. Sign in to LON-CL1 as Adatum\Connie with the password Pa55w.rd.  
2. At a command prompt, run the `gpresult /r` command. RSoP summary results are displayed. The information is very similar to the Summary tab of the RSoP report that was produced by Group Policy Results Wizard. 
3. At the command prompt, type `gpresult /v | more`, and then press Enter. A more detailed RSoP report is produced. Notice that many of the Group Policy settings that were applied by the client are listed in this report. 
4. At the command prompt, type `gpresult /z | more`, and then press Enter. The most detailed RSoP report is produced. 
5. At the command prompt, type `gpresult /h:"%userprofile%\Desktop\RSOP.html"`, and then press Enter. An RSoP report is saved as an HTML file to your desktop. 
6. Open the saved RSoP report from your desktop. Compare the report, its information, and its formatting with the RSoP report that you saved in the previous task. 
7. Sign out of LON-CL1. 

**Task 3: Evaluate GPO results by using Group Policy Modeling Wizard**
1. On LON-DC1, in Group Policy Management Console, open Group Policy Modeling Wizard. 
2. Select Adatum\Connie as the user and LON-CL1 as the computer for modeling. 
3. When prompted, select the Loopback Processing check box, and then click Merge. Even though the Conference Room Settings GPO specifies loopback processing, you must instruct Group Policy Modeling Wizard to include loopback processing in its simulation.  
4. When prompted, on the Alternate Active Directory Paths page, select the Conference Rooms location. You are simulating the effect of LON-CL1 as a conference room computer. 
5. Accept all other options as defaults. 
6. On the Summary tab, scroll to, and if necessary expand, User Details, expand Group Policy Objects, and then expand Applied GPOs. 
7. Check whether the Conference Room Settings GPO applies to Connie as a User policy when she signs in to LON-CL1, if LON-CL1 is in the Conference Rooms OU. 
8. Scroll to, and if necessary expand, User Details\Policies\Administrative Templates\Control Panel \Personalization. 
9. Confirm that the screen saver timeout is 7,200 seconds (2 hours)—the setting configured by the Conference Room Settings GPO that overrides the 10-minute standard configured by the ADATUM Standards GPO. 

**Task 4: Review policy events**
1. Switch to LON-CL1. Sign in as Adatum\Administrator with the password Pa55w.rd. 
2. Open Event Viewer.  
3. Go to the System log located under Windows Logs, and sort events by the Source column. 
4. Locate event 1500, 1501, 1502, or 1503 with Group Policy as the source and review the information that is associated with Group Policy events. 
5. In the console tree, go to the Operational log located under Applications and Services Logs \Microsoft\Windows\Group Policy.  
6. Locate the first event related to the Group Policy refresh that you initiated in the first exercise with the gpupdate command. Review that event and the events that followed it. 
7. Sign out of LON-CL1. 
 
>**Results:** After completing this exercise, you should have used the RSoP tools successfully to verify the correct application of your GPOs, examined Group Policy events, and verified the health of the Group Policy infrastructure. 

### Exercise 2: Troubleshooting GPOs

**Scenario**
A user has opened a Help desk ticket because the screen saver settings do not apply as intended. You have to investigate the issue and make sure that the correct settings apply to the user. You must resolve the reported GPO application problem that Tier 1 help desk staff could not resolve. 

```
- Incident Record
Incident Reference Number: 604531 
Date of Call: July 15
Time of Call: 10:02
User: Connie Vaughn
Status: OPEN 

- Incident Details:
A user reports that the Research configuration does not apply to her anymore. 

- Additional Information:
A user reports that suddenly she has a fixed time of 10 minutes before her screen saver activates. Because of an application that the Research department uses, she is unable to complete her work. 

- Plan of Action: 


- Resolution:

```
 
The main tasks for this exercise are as follows: 
1. Read the Help desk Incident Record and simulate the problem. 
2. Update the Plan of Action section of the Incident Record. 
3. Troubleshoot and resolve the problem. 
4. Prepare for the next module. 

**Task 1: Read the Help desk Incident Record and simulate the problem** 
1. Read Help desk Incident Record 604531 in the exercise scenario. 
2. On LON-DC1, run the E:\Labfiles\Mod05\Mod05-1.ps1 Windows PowerShell script. 

**Task 2: Update the Plan of Action section of the Incident Record**
1. Read the Additional Information section of the Incident Record above. 
2. Update the Plan of Action section of the Incident Record above with your recommendations. 

**Task 3: Troubleshoot and resolve the problem** 
1. On LON-CL1, sign in as Adatum\Connie with the password Pa55w.rd. 
2. Open Control Panel. 
3. In Control Panel, click Change Screen Saver. 
4. Verify that Wait is dimmed and has a value of 10 minutes. 
5. Sign out of LON-CL1. 
6. Using your knowledge of Windows Server GPOs, and the tools available for troubleshooting GPOs, attempt to resolve the problem. 
7. Update the Resolution section of the Incident Record. 
8. If you are unable to resolve the problem, escalate it by asking your instructor for additional guidance. 

    **Note:** You have resolved the issue when Connie Vaughn's screen saver is not locked to 10 minutes.  

>**Results:** After completing this exercise, you will have resolved the GPO application problem. 

**Task 4: Prepare for the next module**
When you finish the lab, revert the virtual machines to their initial state. To do this, perform the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. In the Virtual Machines list, right-click 20742B-LON-DC1, and then click Revert. 
3. In the Revert Virtual Machine dialog box, click Revert. 
4. Repeat steps 2 and 3 for 20742B-LON-CL1. 