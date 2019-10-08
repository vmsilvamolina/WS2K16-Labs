# Lab: Configuring local storage
**Scenario**
Your manager has asked you to add disk space to a file server that is running on a virtual machine. This virtual machine will potentially grow significantly in size in the upcoming months and you might need flexibility in your storage options. Your manager has asked you to optimize the cluster and sector size for virtual machines usage to accommodate large file sizes for storage on virtual machines. You need to assess the best options for storage and ease of expansion for potential future use.

**Objectives**
After completing this lab, you should be able to:
1. Create and manage virtual hard disks.
2. Resize volumes.

**Lab Setup**
1. Estimated Time: 40 minutes
2. Virtual machines: 20740B-LON-DC1, 20740B-LON-SVR1, and 20740B-LON-HOST1
3. User name: Adatum\Administrator
4. Password: Pa55w.rd

For this lab, you will use the available virtual machine environment. Before beginning the lab, you must complete the following steps:
1. On the host computer, start Hyper-V Manager.
2. In Hyper-V Manager, click 20740B-LON-DC1, and then in the Actions pane, click Start.
3. In the Actions pane, click Connect. Wait until the virtual machine starts.
4. Sign in by using the following credentials:
   1. User name: Administrator
   2. Password: Pa55w.rd
   3. Domain: Adatum
5.Repeat steps 2 through 4 for 20740B-LON-SVR1. 

## Exercise 1: Creating and managing volumes
**Scenario**
In the test lab, you start by creating a number of volumes on the installed hard disks. The main tasks for this exercise are as follows:
1. Create a hard disk volume and format for ReFS.
2. Create a mirrored volume. 

**Task 1: Create a hard disk volume and format for ReFS**
1. On LON-SVR1, open Windows PowerShell (Admin).
2. Create a new volume formatted for ReFS by using all the available disk space on Disk 1. Use the following Windows PowerShell cmdlets to complete this process:
   1. List all the available disks that have yet to be initialized: 
   ```Get-Disk | Where-Object PartitionStyle –Eq "RAW"```
   2. Initialize disk 2:  
   ```Initialize-disk 2```
   3. Review the partition table type:  
   ```Get-disk```
   4. Create an ReFS volume by using all the available space on disk 1:
   ```New-Partition -DiskNumber 2 -UseMaximumSize -AssignDriveLetter | Format-Volume  -NewFileSystemLabel "Simple" -FileSystem ReFS```
3. Open File Explorer, and verify that the new drive is created and formatted. What is the drive letter?

**Task 2: Create a mirrored volume**
1. Open Disk Management, and initialize all remaining disks.
2. Create a new volume on Disk 3 and Disk 4 with the following properties:
   1. Disks: Disk 3 and Disk 4
   2. File system: NTFS
   3. Quick format: Yes
   4. Drive letter: M
   5. Volume label: Mirror
   
>**Results:** After completing this exercise, you should have successfully created several volumes.

## Exercise 2: Resizing volumes
**Scenario**
You create a new volume, and then realize that you must resize it. You decide to use **Diskpart.exe** to complete this process.  The main tasks for this exercise are as follows:
1. Create a simple volume and resize it.
2. Shrink a volume.
3. Prepare for the next exercise. 

**Task 1: Create a simple volume and resize it**
1. Switch to Windows PowerShell (Admin) and create a new drive by running the following commands:
   1. Initialize disk 5: ```Initialize-disk 5```
   2. Open diskpart: ```diskpart```. 
   3. List available disks: ```List disk```
   4. Select the appropriate disk: ```Select disk 5```
   5. Make the disk dynamic: ```Convert dynamic```
   6. Create a simple volume on Disk 5: ```Create volume simple size=10000 disk=5```
   7. Assign the drive letter Z: ```Assign letter=z```
   8. Format the volume for NTFS: ```Format```
2. In Disk Management, verify the presence of an NTFS volume on **Disk 5** of size approximately 10 GB.
3. In the Windows PowerShell (Admin) window, run the following command: Extend size 10000 
4. In Disk Management, verify the presence of an NTFS volume on Disk 5 of size approximately 20 GB.

**Task 2: Shrink a volume**
1. In the Windows PowerShell(Admin) window, run the following command: ```Shrink desired=15000```
2. Switch to Disk Management.
3. Verify the presence of an NTFS volume on Disk 5 of size approximately 5 GB.
4. Close the Windows PowerShell (Admin) window.

**Task 3: Prepare for the next exercise**
1. On the host computer, start Hyper-V Manager.
2. In the Virtual Machines list, right-click 20740B-LON-DC1, and then click Revert.
3. In the Revert Virtual Machine dialog box, click Revert.
4. Repeat steps 2 and 3 for 20740B-LON-SVR1.
5. Restart your computer and select 20740B-LON-HOST1 when prompted.
6. Sign in as Administrator with the password Pa55w.rd.

>**Results:** After completing this exercise, you should have successfully resized a volume. 
