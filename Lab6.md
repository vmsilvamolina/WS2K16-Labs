## Lab: Implementing AD DS sites and replication

**Scenario**
A. Datum Corporation has deployed a single AD DS domain, with all the domain controllers located in the London datacenter. As the company has grown and added branch offices with large numbers of users, it has become apparent that the current AD DS environment does not meet the company’s requirements. Users in some branch offices report that it can take a long time for them to sign in to their computers. Access to network resources such as the company’s servers, which are running Microsoft Exchange Server 2016 and Microsoft SharePoint Server 2016, can be slow, and they sporadically fail. As one of the senior network administrators, you are responsible for planning and implementing an AD DS infrastructure that will help address the organization’s business requirements. You are responsible for configuring AD DS sites and replication to optimize the user experience and network utilization within the organization.

**Objectives**
After completing this lab, you will be able to:
- Modify the default site that was created in AD DS.
- Create and configure additional sites and subnets.
- Configure AD DS replication.
- Monitor and troubleshoot AD DS replication. 

**Lab Setup** 
- Estimated Time: 45 minutes
- Virtual machines: 20742B-LON-DC1 and 20742B-TOR-DC1 
- User name: Adatum\Administrator 
- Password: Pa55w.rd 

For this lab, you will use the available virtual machine environment. Before you begin the lab, you must complete the following procedure: 
1. On the host computer, click Start, point to Administrative Tools, and then click Hyper-V Manager. 
2. In Hyper-V Manager, click 20742B-LON-DC1, and then in the Actions pane, click Start. 
3. In the Actions pane, click Connect. Wait until the virtual machine starts. 
4. Sign in by using the following credentials:
- User name: Adatum\Administrator
- Password: Pa55w.rd 
5. Repeat steps 2 through 4 for 20742B-TOR-DC1. 

### Exercise 1: Modifying the default site

**Scenario** 
A. Datum has decided to implement additional AD DS sites to optimize the network utilization for AD DS network traffic. Your first step in implementing the new environment is to install a new domain controller for the Toronto site. You then will reconfigure the default site and assign appropriate IP address subnets to the site. Finally, your task is to change the name of the default site to LondonHQ and associate it with the 172.16.0.0/24 IP subnet, which is the subnet range for the London head office. 

The main tasks for this exercise are as follows: 
1. Install the Toronto domain controller. 
2. Rename the default site. 
3. Configure IP subnets that are associated with the default site. 

**Task 1: Install the Toronto domain controller** 
1. On TOR-DC1, start Server Manager and install Active Directory Domain Services. 
2. When the AD DS binaries have installed, use the Active Directory Domain Services Configuration Wizard to install and configure TOR-DC1 as an additional domain controller for Adatum.com. Under Type the Directory Services Restore Mode (DSRM) password, type Pa55w.rd in both the Password and Confirm password boxes. Let the server restart as indicated. 
3. After the server restarts, sign in as Adatum\Administrator with the password Pa55w.rd. 

**Task 2: Rename the default site** 
1. If necessary, on LON-DC1, open the Server Manager console. 
2. 2. Open Active Directory Sites and Services, and then rename the Default-First-Site-Name site to LondonHQ. 
3. Verify that both LON-DC1 and TOR-DC1 are members of the LondonHQ site.

**Task 3: Configure IP subnets that are associated with the default site**
1. If necessary, on LON-DC1, open the Server Manager console, and then open Active Directory Sites and Services. 
2. Create a new subnet with the following configuration: 
- Prefix: 172.16.0.0/24 
- Site object: LondonHQ 
 
>**Results:** After completing this exercise, you should have successfully reconfigured the default site and assigned IP address subnets to the site. 

### Exercise 2: Creating additional sites and subnets

**Scenario**
The next step you take to implement the AD DS site design is to configure the new AD DS site. The first site that you must implement is the Toronto site for the North American datacenter. The network team in Toronto also wants to dedicate a site named TestSite in the Toronto datacenter. You have been instructed that the Toronto IP subnet address is 172.16.1.0/24, and the test network IP subnet address is 172.16.100.0/24.

The main tasks for this exercise are as follows: 
1. Create the AD DS sites for Toronto. 
2. Create IP subnets that are associated with the Toronto sites.

**Task 1: Create the AD DS sites for Toronto** 
1. If necessary, on LON-DC1, open the Server Manager console, and then open Active Directory Sites and Services. 
2. Create a new site with the following configuration: 
- Name: Toronto 
- Site link object: DEFAULTIPSITELINK 
1. Create another new site with the following configuration:
- Name: TestSite 
- Site link object: DEFAULTIPSITELINK

**Task 2: Create IP subnets that are associated with the Toronto sites** 
1. If necessary, on LON-DC1, open Active Directory Sites and Services. 
2. Create a new subnet with the following configuration: 
- Prefix: 172.16.1.0/24 
- Site object: Toronto 
1. Create another new subnet with the following configuration: 
- Prefix: 172.16.100.0/24 
- Site object: TestSite 
4. In the navigation pane, click the Subnets folder. Verify in the details pane that the two subnets are created and associated with their appropriate site. 
 
>**Results:** After completing this exercise, you should have successfully created two additional sites representing the IP subnet addresses in Toronto. 

### Exercise 3: Configuring AD DS replication

**Scenario**
Now that the AD DS sites have been configured for Toronto, your next step is to configure the site links to manage replication between the sites and then to move the TOR-DC1 domain controller to the Toronto site. Currently, all sites belong to DEFAULTIPSITELINK. You need to modify site linking so that LondonHQ and Toronto belong to one common site link called LON-TOR. You should configure this link to replicate every hour. Additionally, you should link the TestSite site only to the Toronto site by using a site link named TOR-TEST. Replication should not be available from the Toronto site to the TestSite site during working hours from 9 AM to 3 PM. You then will use tools to monitor replication between the sites. 

The main tasks for this exercise are as follows: 
1. Configure site links between AD DS sites. 
2. Move TOR-DC1 to the Toronto site. 
3. Monitor AD DS site replication.

**Task 1: Configure site links between AD DS sites** 
1. If necessary, on LON-DC1, open Active Directory Sites and Services. 
2. Create a new IP-based site link with the following configuration: 
- Name: TOR-TEST 
- Sites: Toronto, TestSite 
- Modify the schedule to only allow replication from Monday 9 AM to Friday 3 PM 
1. Rename DEFAULTIPSITELINK, and then configure it with the following settings: 
- Name: LON-TOR 
- Sites: LondonHQ, Toronto 
- Replication: Every 60 minutes

**Task 2: Move TOR-DC1 to the Toronto site** 
1. If necessary, on LON-DC1, open Active Directory Sites and Services. 
2. Move TOR-DC1 from the LondonHQ site to the Toronto site. 
3. Verify that TOR-DC1 is located under the Servers node in the Toronto site. 

**Task 3: Monitor AD DS site replication** 
1. On LON-DC1, on the taskbar, click the Windows PowerShell icon. 
2. Use the following commands to monitor site replication: 
`Repadmin /kcc`
This command recalculates the inbound replication topology for the server: 
`Repadmin /showrepl`
Verify that the last replication with TOR-DC1 was successful: 
`Repadmin /bridgeheads`
This command displays the bridgehead servers for the site topology: 
`Repadmin /replsummary`
This command displays a summary of replication tasks. Verify that no errors appear: 
`DCDiag /test:replications` 
Verify that all connectivity and replication tests pass successfully. 3. Switch to TOR-DC1, and then repeat the commands to view information from the TOR-DC1 perspective. 
 
>**Results:** After completing this exercise, you should have successfully configured site links and monitored replication. 

### Exercise 4: Monitoring and troubleshooting AD DS replication

**Scenario**
After AD DS sites and replication are established, A. Datum experiences replication issues. You must use monitoring and troubleshooting tools to diagnose the issue and resolve it. The main tasks for this exercise are as follows: 
1. Produce an error. 
2. Monitor AD DS site replication. 
3. Troubleshoot AD DS replication. 
4. Prepare for the next module.

**Task 1: Produce an error** 
1. On LON-DC1, open Server Manager, and then open Active Directory Sites and Services. 
2. In Active Directory Sites and Services, replicate TOR-DC1 with LON-DC1 from the LondonHQ site. 
3. In Windows PowerShell, run:  
`Get-ADReplicationUpToDatenessVectorTable –Target “adatum.com”`
4. Observe the results, and then note the date and time of the most recent replication event. 
5. Go to TOR-DC1, open Windows PowerShell, and then run the following Windows PowerShell commands:  
`CD \Labfiles\Mod04`
`.\Mod04Ex4.ps1`

**Task 2: Monitor AD DS site replication** 
1. On TOR-DC1, in Active Directory Sites and Services, replicate LON-DC1 with TOR-DC1 from the Toronto site. 
2. On TOR-DC1, in Windows PowerShell, run the following cmdlets, and then observe the results: 
`Get-ADReplicationUpToDatenessVectorTable –Target “adatum.com”`
`Get-AdReplicationSubnet –filter *`
`Get-AdReplicationSiteLink–filter *`

**Task 3: Troubleshoot AD DS replication** 
1. On TOR-DC1, in Windows PowerShell, determine the IP address settings for the computer, and then run the following cmdlet: 
`Get-DnsClient | Set-DnsClientServerAddress -ServerAddresses ("172.16.0.10","172.16.0.25")`
Ensure that the IP address settings are correct. 
2. Go to Active Directory Sites and Services, and then replicate LON-DC1 with TOR-DC1 from the Toronto site. Review the objects to determine if any are missing. 
3. On TOR-DC1, open File Explorer. Browse to C:\Labfiles\Mod04. 
4. Right-click the Mod04EX4Fix.ps1 file, and then select Run with PowerShell. 
5. In Active Directory Sites and Services, examine all the objects that you created earlier. Ensure that the site link has been created in the Inter-Site Transports node, and subnets have been created in the Subnets node. 
6. On LON-DC1 and TOR-DC1, close all open windows, and then sign out of both virtual machines. 
 
>**Results:** After completing this exercise, you should have successfully diagnosed and resolved replication issues. 

**Task 4: Prepare for the next module**
When you finish the lab, revert the virtual machines to their initial state. To do this, complete the following steps: 
1. On the host computer, start Hyper-V Manager. 
2. On the Virtual Machines list, right-click 20742B-LON-DC1, and then click Revert. 
3. In the Revert Virtual Machine dialog box, click Revert. 
4. Repeat steps 2 and 3 for 20742B-TOR-DC1.