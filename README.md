# Network File Shares and Permissions

<p align="center">
<img src="https://github.com/user-attachments/assets/94191e30-fbe0-406a-8c48-051552c336ee" alt="Lab 7 Header" width="80%"/>
</p>

## Project Overview

This project demonstrates how businesses control access to shared network resources using Windows file share permissions and Active Directory security groups. Using the same Active Directory infrastructure from previous labs, four shared folders were created on the Domain Controller with different permission levels and tested from a client machine logged in as a standard domain user. A custom ACCOUNTANTS security group was then created in Active Directory and used to grant selective access to a restricted folder, proving how organizations manage file access at scale without assigning permissions to individual users. Real troubleshooting was required during this lab when the hostname path failed and the IP address had to be used instead, reinforcing that practical IT support requires methodical diagnosis not just following steps.

# Environments and Technologies Used

* Microsoft Azure (Virtual Machines, Active Directory Environment)
* Windows Server 2022 (DC-1: Domain Controller and File Server)
* Windows 10 Pro (Client-1)
* Active Directory Users and Computers
* Windows File Explorer
* PowerShell
* Remote Desktop Protocol (RDP)

# Operating Systems Used

* Windows Server 2022 Datacenter Azure Edition (DC-1)
* Windows 10 Pro (Client-1)

# Project Objectives

* Create four shared folders on DC-1 with different permission levels
* Configure share permissions for Domain Users and Domain Admins
* Test folder access from Client-1 as a standard domain user
* Observe access denied behavior on restricted folders
* Create an ACCOUNTANTS security group in Active Directory
* Assign the ACCOUNTANTS group permissions to the accounting folder
* Verify access is denied before and granted after adding a user to the group
* Delete all Azure resources after lab completion

# High-Level Steps

1. Create read-access, write-access, no-access and accounting folders on DC-1 C drive
2. Share each folder and configure permissions for the appropriate groups
3. Log into Client-1 as a standard domain user and navigate to the shared folders
4. Test access on each folder and observe permission enforcement
5. Create the ACCOUNTANTS security group in Active Directory
6. Assign the ACCOUNTANTS group Read/Write permissions on the accounting folder
7. Verify accounting folder access is denied before adding user to group
8. Add the domain user to the ACCOUNTANTS group on DC-1
9. Log back into Client-1 and verify accounting folder is now accessible
10. Delete all Azure resources after completion

# Lab Steps

## Step 1: Four Folders Created on DC-1 C Drive

<p align="center">
<img src="https://github.com/user-attachments/assets/ef8626a3-072b-4e55-9e5e-b33d2112258c" alt="Four Folders Created on DC-1 C Drive" width="80%"/>
</p>

Created four folders on the C drive of DC-1 named read-access, write-access, no-access and accounting. These folders represent the different types of shared network resources a business might maintain — some accessible to all employees, some restricted to specific teams and some completely locked down to administrators only. Organizing shared resources this way is standard practice in any Windows based business environment where different departments need different levels of access to different files.

## Step 2: Folder Permissions Configured

<p align="center">
<img src="https://github.com/user-attachments/assets/324aa5c7-da1f-46eb-b381-4c55ea0e00ec" alt="Folder Permissions Configured" width="80%"/>
</p>

Configured share permissions on the write-access folder granting Domain Users Read/Write access through the Advanced Sharing settings. Each folder was configured with the following permissions: read-access granted Domain Users Read only, write-access granted Domain Users Read/Write, no-access granted Domain Admins Read/Write only completely excluding standard users, and accounting was reserved for the ACCOUNTANTS security group configured later. Setting permissions at the share level is how businesses enforce data security ensuring employees can only access and modify the resources their role requires.

## Step 3: Shared Folders Visible from Client-1

<p align="center">
<img src="https://github.com/user-attachments/assets/4720e2f9-e88c-4436-9b9d-b75bdb1540b6" alt="Shared Folders Visible from Client-1" width="80%"/>
</p>

Navigated to the shared folders from Client-1 by opening the Run dialog and typing the UNC path using DC-1's IP address directly. During troubleshooting the hostname path failed with "Windows cannot find \\dc-1" so the IP address \\10.0.0.4 was used instead. Before switching to the IP address the net share command was run on DC-1 in PowerShell to confirm all shares were active and ping 10.0.0.4 was run on Client-1 to confirm network connectivity. This methodical diagnosis confirmed the issue was hostname resolution not the shares themselves. All four folders are now visible from Client-1 exactly as they would appear to an employee accessing a company file server.

## Step 4: Access Denied on No-Access Folder

<p align="center">
<img src="https://github.com/user-attachments/assets/43911b89-751f-408b-8e5f-8178ae96d7e9" alt="Access Denied on No-Access Folder" width="80%"/>
</p>

Attempted to open the no-access folder from Client-1 as a standard domain user and received a Windows permission denied error. This confirms the share permissions are working correctly. The no-access folder was configured with Read/Write permissions for Domain Admins only meaning standard domain users are completely blocked from entering. In a real business environment this type of folder represents sensitive administrative resources like payroll data, HR records or system configuration files that only IT administrators or senior management should access.

## Step 5: Successfully Reading from Read-Access Folder

<p align="center">
<img src="https://github.com/user-attachments/assets/722596f5-3e33-4389-9022-f57d45c4ddc5" alt="Successfully Reading from Read-Access Folder" width="80%"/>
</p>

Successfully opened the read-access folder from Client-1 as a standard domain user confirming read permissions are working correctly for Domain Users. The folder is accessible and its contents can be viewed but files cannot be created or modified because only Read permission was granted. Read only access is commonly used in business environments for shared reference materials, policy documents, templates and resources that all employees need to view but only specific administrators should be able to update.

## Step 6: Successfully Writing to Write-Access Folder

<p align="center">
<img src="https://github.com/user-attachments/assets/0591c225-c086-4f00-bc6a-45f7c850a87f" alt="Successfully Writing to Write-Access Folder" width="80%"/>
</p>

Successfully opened the write-access folder from Client-1 and confirmed Read/Write access is working for Domain Users. A new file was created inside the folder proving that standard domain users can both read existing files and create new content. Read/Write shared folders are commonly used in businesses for collaborative workspaces, team project folders and shared document libraries where multiple employees need to contribute and modify files as part of their daily work.

## Step 7: ACCOUNTANTS Security Group Created in ADUC

<p align="center">
<img src="https://github.com/user-attachments/assets/7a960499-0f05-40fa-b271-fb037341825d" alt="ACCOUNTANTS Security Group Created in ADUC" width="80%"/>
</p>

Created a new security group called ACCOUNTANTS in Active Directory Users and Computers on DC-1. Security groups are how businesses manage file and resource access at scale without assigning permissions to individual user accounts. Instead of granting accounting folder access to each accountant one by one the ACCOUNTANTS group is granted access once and any user added to that group automatically inherits those permissions. This approach makes onboarding new employees, managing role changes and revoking access significantly more efficient and consistent across the organization.

## Step 8: Access Denied to Accounting Folder Before Group Membership

<p align="center">
<img src="https://github.com.user-attachments/assets/3b1d1f56-da3f-424f-960c-20aaca7242c6" alt="Access Denied to Accounting Folder Before Group Membership" width="80%"/>
</p>

Attempted to access the accounting folder from Client-1 as the standard domain user before being added to the ACCOUNTANTS group and received a permission denied error. This confirms the accounting folder is correctly restricted to ACCOUNTANTS group members only. This before screenshot is critical because it sets up the proof of concept — the same user who cannot access the folder right now will gain access simply by being added to the correct security group, without any changes to the folder permissions themselves.

## Step 9: User Added to ACCOUNTANTS Group on DC-1

<p align="center">
<img src="https://github.com/user-attachments/assets/c85f65b0-4399-4687-a418-d318db5bac26" alt="User Added to ACCOUNTANTS Group on DC-1" width="80%"/>
</p>

Added the standard domain user to the ACCOUNTANTS security group in Active Directory Users and Computers on DC-1 by opening the group properties and adding the user through the Members tab. This single administrative action on the Domain Controller is all that is required to grant the user access to the accounting folder. In a real business environment this represents the process of granting a new employee access to department resources when they join a team or change roles, all managed centrally through Active Directory without touching the file server directly.

## Step 10: Successfully Accessing Accounting Folder After Group Membership

<p align="center">
<img src="https://github.com/user-attachments/assets/f4d3828c-f222-4dab-b9ff-fc3035cafd4e" alt="Successfully Accessing Accounting Folder After Group Membership" width="80%"/>
</p>

After logging out of Client-1 and logging back in as the same domain user the accounting folder is now fully accessible. The folder that returned a permission denied error before adding the user to the ACCOUNTANTS group is now open and accessible without any changes to the folder permissions themselves. This is the end to end proof of how security group based access control works in a real business environment. A single administrative action in Active Directory on the Domain Controller instantly grants or revokes access to shared resources across the entire network.

## Step 11: Resource Group Deletion Confirmation

<p align="center">
<img src="https://github.com/user-attachments/assets/4d9ceaf0-a3ef-45df-9c62-187fcf02cf9c" alt="Resource Group Deletion Confirmation" width="80%"/>
</p>

Deleted all Azure resources after completing the final lab in the Course Careers IT program. This deletion marks the official close of the entire Active Directory and networking lab series spanning Labs 5, 6 and 7 which all used the same DC-1 and Client-1 virtual machines. Consistently practicing cloud resource cleanup across every lab reflects the cost-conscious discipline required of IT professionals and cloud administrators managing real business infrastructure. Unmanaged cloud resources accumulate charges daily and proper cleanup is a real and valued skill in any organization running cloud based infrastructure.

# Key Skills Demonstrated

* Network file share creation and configuration on Windows Server
* Share permission management for Domain Users and Domain Admins
* UNC path navigation and network share access from domain joined client machines
* Active Directory security group creation and membership management
* Permission based access control using security groups
* Real world troubleshooting: diagnosed hostname resolution failure using net share and ping then resolved by switching to IP address
* Cloud cost management: consistent resource deletion across all labs

# Business Applications

File share permissions are one of the most fundamental and frequently managed aspects of IT administration in any Windows based organization. Every business needs to control who can read, write and access different types of files. Sensitive financial data should only be accessible to the finance team. HR records should only be accessible to HR administrators. Company wide reference documents should be readable by everyone but editable only by designated owners. This lab demonstrates exactly how businesses implement that structure using Windows file share permissions combined with Active Directory security groups. The security group approach is particularly important at scale because it allows IT administrators to manage access for entire departments through a single group rather than configuring permissions for each individual user. When an employee joins the accounting team they are added to the ACCOUNTANTS group and instantly gain access to all accounting resources. When they leave the team they are removed from the group and instantly lose that access. No individual file or folder permissions need to be touched.

# Lessons Learned

The most valuable moment in this lab came from the troubleshooting required to access the shared folders from Client-1. The hostname path failed with a Windows cannot find error despite the shares being confirmed active through net share on DC-1 and network connectivity confirmed through a successful ping. Switching to the IP address resolved the issue immediately. This experience reinforced a core IT support principle: when one approach fails do not give up, isolate the variables methodically. Confirming the shares existed, confirming the network was working and then identifying hostname resolution as the specific failure point is exactly the kind of systematic thinking that separates effective IT professionals from those who only know how to follow instructions. The fix itself was simple but finding it required understanding what each tool confirms and what each failure means.
