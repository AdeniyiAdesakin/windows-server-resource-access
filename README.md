# Windows Server Printer Deployment and Secure File Sharing

**Print Server | Group Policy | SMB File Sharing | NTFS Permissions | Drive Mapping**

## Project Overview

I configured centralized access to a shared network printer and a departmental folder in a Windows Server Active Directory environment.

For printer access, I installed the Print Server role, added an HP Universal Printing PCL 6 driver, created a TCP/IP printer queue, shared the printer, and deployed it to domain clients through Group Policy.

For file access, I created an SMB share for the HR department, assigned permissions to an Active Directory security group, and used Group Policy Preferences to map the shared folder on authorized domain computers.

This project demonstrates practical Windows Server resource administration, Group Policy deployment, permission management, and client-side validation.

## Business Scenario

An organization needs a centralized way to provide employees with access to shared printers and departmental files. Configuring these resources manually on every workstation would take time, create inconsistent settings, and increase the effort required to support users.

To meet this requirement, I used Windows Server and Group Policy to deploy the resources from a central location. The printer was assigned through a printer GPO, while access to the departmental folder was controlled through an Active Directory security group and a mapped-drive GPO.

## Project Objectives

- Install and verify the Windows Print Server role.
- Add a vendor-supplied x64 printer driver.
- Create and share a TCP/IP printer queue.
- Deploy the shared printer through Group Policy.
- Publish the printer in Active Directory.
- Create a departmental SMB file share.
- Configure folder access for an Active Directory security group.
- Create a mapped-drive preference through Group Policy.
- Limit the drive mapping with item-level targeting.
- Validate the printer and mapped drive from a domain client.

## Lab Environment

| Component | Configuration |
| --- | --- |
| Active Directory domain | `adeniyi.com` |
| Windows Server | `ADDS-Server` |
| Domain client | `WIN10-CLIENT1` |
| Printer GPO | `Printer_Policy` |
| Printer target OU | `Toronto` |
| Printer driver | HP Universal Printing PCL 6 v7.2.0, x64 |
| Shared-folder path | `C:\Shares\O_HR_Share` |
| Shared-folder UNC path | `\\ADDS-Server\O_HR_Share` |
| File-access security group | `ADENIYI\O_HR` |
| Drive-map GPO | `O_HR Mapped drive` |
| Drive-map target OU | `Ottawa` |
| Administration tools | Server Manager, Print Management, Group Policy Management |

## Skills Demonstrated

- Windows Server administration
- Print Server role configuration
- Printer-driver installation
- TCP/IP printer deployment
- Group Policy administration
- Active Directory printer publishing
- SMB file sharing
- NTFS permission configuration
- Active Directory security-group administration
- Group Policy Preferences
- Item-level targeting
- Windows client validation
- Technical documentation

## Implementation

## Part 1: Deploying a Shared Printer with Group Policy

### 1. Created the Printer Group Policy Object

In **Group Policy Management**, I created a new GPO named `Printer_Policy`.

<p align="center">
  <img src="https://i.imgur.com/oiGO6Op.png" width="750" alt="Creating a new Group Policy Object for printer deployment">
</p>

<p align="center">
  <img src="https://i.imgur.com/djiaZyB.png" width="750" alt="Naming the new Group Policy Object Printer Policy">
</p>

### 2. Linked the Printer GPO to the Target OU

I linked `Printer_Policy` to the `Toronto` organizational unit so that the printer deployment settings could apply to the accounts within that OU.

<p align="center">
  <img src="https://i.imgur.com/1qYwc2u.png" width="750" alt="Linking an existing Group Policy Object to the Toronto OU">
</p>

<p align="center">
  <img src="https://i.imgur.com/jHHGXMO.png" width="750" alt="Selecting Printer Policy from the available Group Policy Objects">
</p>

### 3. Verified the Print Server Role

From **Server Manager > Tools**, I opened **Print Management**.

<p align="center">
  <img src="https://i.imgur.com/yyoBnpo.png" width="750" alt="Opening Print Management from Windows Server Manager">
</p>

I also verified that **Print and Document Services > Print Server** was installed. If the role is not available, it can be installed through **Add Roles and Features**.

<p align="center">
  <img src="https://i.imgur.com/vfX0fsz.png" width="750" alt="Verifying the Print Server role under Print and Document Services">
</p>

### 4. Downloaded the Printer Driver

For the lab, I downloaded the **HP Universal Print Driver for Windows PCL 6 (64-bit)** from HP.

<p align="center">
  <img src="https://i.imgur.com/kCHFsdr.png" width="750" alt="Downloading the HP Universal Print Driver for Windows PCL 6">
</p>

### 5. Added the Printer Driver to the Server

In Print Management, I expanded **Print Servers > ADDS-Server > Drivers**, right-clicked **Drivers**, and selected **Add Driver**.

<p align="center">
  <img src="https://i.imgur.com/h1Enztr.png" width="750" alt="Selecting Add Driver from Windows Print Management">
</p>

I started the Add Printer Driver Wizard.

<p align="center">
  <img src="https://i.imgur.com/1kcm27j.png" width="750" alt="Starting the Add Printer Driver Wizard">
</p>

I selected the `x64` processor architecture.

<p align="center">
  <img src="https://i.imgur.com/Gs4KuPZ.png" width="750" alt="Selecting the x64 processor architecture for the printer driver">
</p>

Because the driver was downloaded from the manufacturer, I selected **Have Disk**.

<p align="center">
  <img src="https://i.imgur.com/C5cpy2H.png" width="750" alt="Selecting Have Disk in the Printer Driver Selection window">
</p>

I browsed to the extracted HP driver files.

<p align="center">
  <img src="https://i.imgur.com/HE2oAJj.png" width="750" alt="Browsing for the extracted printer driver files">
</p>

<p align="center">
  <img src="https://i.imgur.com/cgBNEzS.png" width="750" alt="Selecting the HP Universal Print Driver folder">
</p>

<p align="center">
  <img src="https://i.imgur.com/DuEBJ45.png" width="750" alt="Confirming the HP Universal Print Driver installation path">
</p>

I selected **HP Universal Printing PCL 6 (v7.2.0)**.

<p align="center">
  <img src="https://i.imgur.com/5I5T7jR.png" width="750" alt="Selecting HP Universal Printing PCL 6 version 7.2.0">
</p>

The wizard confirmed that the x64 printer driver was added successfully.

<p align="center">
  <img src="https://i.imgur.com/odBu2ud.png" width="750" alt="Completing the Add Printer Driver Wizard">
</p>

### 6. Created the TCP/IP Printer Queue

Under **Print Servers > ADDS-Server > Printers**, I selected **Add Printer**.

<p align="center">
  <img src="https://i.imgur.com/y29KTWD.png" width="750" alt="Adding a printer through Windows Print Management">
</p>

I selected **Add a TCP/IP or Web Services Printer by IP address or hostname**.

<p align="center">
  <img src="https://i.imgur.com/Afizfil.png" width="750" alt="Selecting TCP IP printer installation by address or hostname">
</p>

I entered the printer's network address and continued with the installation.

<p align="center">
  <img src="https://i.imgur.com/QHaxKPI.png" width="750" alt="Entering the network printer IP address">
</p>

When automatic detection did not identify the printer, I selected **Hewlett Packard Jet Direct** as the device type.

<p align="center">
  <img src="https://i.imgur.com/sUiN0mE.png" width="750" alt="Selecting Hewlett Packard Jet Direct as the printer device type">
</p>

I selected the HP Universal Printing PCL 6 driver that had already been installed on the server.

<p align="center">
  <img src="https://i.imgur.com/TwgNJs8.png" width="750" alt="Selecting the installed HP Universal Printing PCL 6 driver">
</p>

### 7. Shared the Printer

I enabled **Share this printer** and assigned a descriptive share name.

<p align="center">
  <img src="https://i.imgur.com/owdf9eM.png" width="750" alt="Configuring the printer name and sharing settings">
</p>

I reviewed the queue configuration before completing the wizard.

<p align="center">
  <img src="https://i.imgur.com/6C5Ms8v.png" width="750" alt="Reviewing the network printer configuration">
</p>

The Network Printer Installation Wizard confirmed that the printer was installed successfully.

<p align="center">
  <img src="https://i.imgur.com/Qvqi5Ez.png" width="750" alt="Completing the Network Printer Installation Wizard">
</p>



### 8. Deployed the Printer Through Group Policy

In Print Management, I right-clicked the shared printer and selected **Deploy with Group Policy**.

<p align="center">
  <img src="https://i.imgur.com/IAZAX9j.png" width="750" alt="Selecting Deploy with Group Policy for the shared printer">
</p>

I selected `Printer_Policy` and added the printer connection to the GPO.

<p align="center">
  <img src="https://i.imgur.com/Agd732q.png" width="750" alt="Adding the shared printer to Printer Policy">
</p>

The captured configuration contains both per-user and per-machine entries. The required deployment type should be selected according to whether the printer is assigned to users or computers.

### 9. Refreshed Group Policy

I ran the original PowerShell command to force a Group Policy refresh:

```powershell
gpupdate /force
```

The output confirmed that both Computer Policy and User Policy updated successfully.

<p align="center">
  <img src="https://i.imgur.com/oKzW8z2.png" width="750" alt="Running gpupdate force and completing the Group Policy refresh">
</p>

### 10. Validated the Printer on the Client

On the Windows client, I opened **Control Panel > Hardware and Sound > Devices and Printers**. The HP shared printer appeared with `ADDS-Server` identified as the print server.

<p align="center">
  <img src="https://i.imgur.com/zpS03rF.png" width="750" alt="Shared HP printer displayed on the Windows domain client">
</p>

### 11. Published the Printer in Active Directory

In the printer's properties, I enabled **List in the directory**. This publishes the shared printer in Active Directory so domain users can locate it through directory searches.

<p align="center">
  <img src="https://i.imgur.com/QVvhTdo.png" width="750" alt="Publishing the shared printer in Active Directory">
</p>

## Part 2: Configuring a Shared Folder and Mapped Drive

### 1. Started the New Share Wizard

In Server Manager, I opened **File and Storage Services**.

<p align="center">
  <img src="https://i.imgur.com/5XpIrPk.png" width="750" alt="Opening File and Storage Services in Server Manager">
</p>

From **Shares**, I selected **Tasks > New Share**.

<p align="center">
  <img src="https://i.imgur.com/c7TjK48.png" width="750" alt="Starting the New Share Wizard from Server Manager">
</p>

### 2. Selected the SMB Share Profile

I selected **SMB Share - Quick** as the file-share profile.

<p align="center">
  <img src="https://i.imgur.com/L0W6IDO.png" width="750" alt="Selecting the SMB Share Quick profile">
</p>

### 3. Selected the Server and Storage Location

I selected `ADDS-Server` and the `C:` volume as the location for the share.

<p align="center">
  <img src="https://i.imgur.com/bZ4gZSn.png" width="750" alt="Selecting ADDS Server and the C drive for the new share">
</p>

### 4. Configured the Share Name and Path

I configured the following share information:

| Setting | Value |
| --- | --- |
| Share name | `O_HR_Share` |
| Local path | `C:\Shares\O_HR_Share` |
| UNC path | `\\ADDS-Server\O_HR_Share` |

<p align="center">
  <img src="https://i.imgur.com/YRPferH.png" width="750" alt="Configuring the O HR Share name and paths">
</p>

### 5. Reviewed the SMB Share Settings

I retained the selected caching setting and continued to the permissions page.

<p align="center">
  <img src="https://i.imgur.com/W4NVIFu.png" width="750" alt="Reviewing the SMB share configuration settings">
</p>

### 6. Configured the Folder Permissions

In **Advanced Security Settings**, I disabled inherited permissions and converted the inherited entries into explicit permissions.

<p align="center">
  <img src="https://i.imgur.com/b9QOtmI.png" width="750" alt="Disabling inherited permissions for the O HR Share folder">
</p>

I removed the unnecessary user entries and selected **Add** to create a permission entry for the HR security group.

<p align="center">
  <img src="https://i.imgur.com/eeW5X0t.png" width="750" alt="Adding a permission entry to the O HR Share folder">
</p>

I selected the `O_HR` Active Directory security group.

<p align="center">
  <img src="https://i.imgur.com/G9LisVZ.png" width="750" alt="Selecting the O HR Active Directory security group">
</p>

I gave `O_HR` Full Control over the folder, subfolders, and files.

<p align="center">
  <img src="https://i.imgur.com/bHSiBdG.png" width="750" alt="Granting the O HR security group folder permissions">
</p>

I reviewed the resulting NTFS access-control entries.

<p align="center">
  <img src="https://i.imgur.com/DGok4C7.png" width="750" alt="Reviewing the completed O HR Share NTFS permissions">
</p>

P.S - I am aware that for normal departmental file access, `SYSTEM` and `Administrators` should retain Full Control while the department group is usually assigned **Modify**. Full Control should be limited to accounts that need to change permissions or ownership.

### 7. Created the SMB Share

The permissions page showed the selected group and folder permissions.

<p align="center">
  <img src="https://i.imgur.com/CdM9u9k.png" width="750" alt="Reviewing share and folder permissions in the New Share Wizard">
</p>

I reviewed the configuration and selected **Create**.

<p align="center">
  <img src="https://i.imgur.com/NK6NoyW.png" width="750" alt="Confirming the O HR Share configuration">
</p>

The wizard confirmed that the SMB share and permissions were created successfully.

<p align="center">
  <img src="https://i.imgur.com/wUVEfpL.png" width="750" alt="Successful creation of the SMB share and permissions">
</p>

### 8. Created the Mapped-Drive GPO

In Group Policy Management, I created a new GPO.

<p align="center">
  <img src="https://i.imgur.com/FUqISyB.png" width="750" alt="Creating a new Group Policy Object for the mapped drive">
</p>

I named the GPO `O_HR Mapped drive`.

<p align="center">
  <img src="https://i.imgur.com/8ak3JxJ.png" width="750" alt="Naming the O HR Mapped drive Group Policy Object">
</p>

I right-clicked the new GPO and selected **Edit**.

<p align="center">
  <img src="https://i.imgur.com/2h4r58z.png" width="750" alt="Editing the O HR Mapped drive Group Policy Object">
</p>

### 9. Created the Drive Maps Preference

In Group Policy Management Editor, I navigated to:
`User Configuration > Preferences > Windows Settings > Drive Maps`

I right-clicked **Drive Maps** and selected **New > Mapped Drive**.

<p align="center">
  <img src="https://i.imgur.com/cIpPKMX.png" width="750" alt="Creating a new mapped drive under Group Policy Preferences">
</p>

### 10. Configured the Mapped Drive

I configured the mapped-drive preference with the following values:

| Setting | Value |
| --- | --- |
| Action | Update |
| Location | `\\ADDS-Server\O_HR_Share` |
| Reconnect | Enabled |
| Label | `O_HR` |
| Drive letter configured | `H:` |

<p align="center">
  <img src="https://i.imgur.com/V24jCjR.png" width="750" alt="Configuring the O HR mapped drive properties">
</p>

### 11. Enabled Item-Level Targeting

On the **Common** tab, I enabled **Item-level targeting** and opened the Targeting Editor.

<p align="center">
  <img src="https://i.imgur.com/HpFlamA.png" width="750" alt="Enabling item-level targeting for the mapped drive">
</p>

I selected **Security Group** as the targeting condition.

<p align="center">
  <img src="https://i.imgur.com/R4gD0vQ.png" width="750" alt="Selecting Security Group in the Group Policy Targeting Editor">
</p>

I opened the group selector.

<p align="center">
  <img src="https://i.imgur.com/uziQgNA.png" width="750" alt="Opening the security group selector in the Targeting Editor">
</p>

I selected the `O_HR` security group.

<p align="center">
  <img src="https://i.imgur.com/hNtkAiE.png" width="750" alt="Selecting the O HR group for item-level targeting">
</p>

I selected **Computer in group** for the targeting condition.

<p align="center">
  <img src="https://i.imgur.com/sepC9Gl.png" width="750" alt="Targeting computers that belong to the O HR security group">
</p>

I applied and saved the mapped-drive preference.

<p align="center">
  <img src="https://i.imgur.com/nEl0hDl.png" width="750" alt="Saving the O HR mapped drive preference">
</p>

### 12. Linked the Drive-Map GPO

In Group Policy Management, I right-clicked the `Ottawa` OU and selected **Link an Existing GPO**.

<p align="center">
  <img src="https://i.imgur.com/fJ6Fraw.png" width="750" alt="Linking an existing GPO to the Ottawa OU">
</p>

I selected `O_HR Mapped drive` and completed the link.

<p align="center">
  <img src="https://i.imgur.com/b9FjG7G.png" width="750" alt="Selecting the O HR Mapped drive Group Policy Object">
</p>


### 13. Validated the Mapped Drive

On the domain client, I opened File Explorer and confirmed that the departmental share appeared as a mapped network location.

<p align="center">
  <img src="https://i.imgur.com/Isb0jxo.png" width="750" alt="O HR Share displayed as a mapped network drive on the domain client">
</p>

P.S - I changed the drive letter in between the lab, hence why the the `H` at the beginning  and the `Z` at the end of the project. 

## Validation Results

| Validation item | Result |
| --- | --- |
| Print Server role available | Passed |
| HP Universal Printing PCL 6 x64 driver installed | Passed |
| TCP/IP printer queue created | Passed |
| Printer shared through `ADDS-Server` | Passed |
| Printer added to `Printer_Policy` | Passed |
| Computer and User Group Policy refreshed | Passed |
| Shared printer displayed on the domain client | Passed |
| Printer published in Active Directory | Passed |
| `O_HR_Share` created successfully | Passed |
| `O_HR` security group added to the folder ACL | Passed |
| Drive Maps preference created | Passed |
| Item-level targeting configured | Passed |
| Drive-map GPO linked to an OU | Passed |
| Mapped network location displayed on the client | Passed |

## Key Takeaways

This project demonstrated how Windows Server and Group Policy can provide centralized access to shared organizational resources.

The printer deployment reduced the need to configure the printer manually on each workstation. The SMB share and mapped-drive configuration demonstrated how Active Directory groups, NTFS permissions, and Group Policy Preferences can work together to provide department-specific file access.

The project also reinforced the importance of matching GPO scope to the correct user or computer objects and using consistent printer addresses, drive letters, group membership, and permissions throughout a deployment.


## Related Projects

- [Active Directory Domain Services and Windows Client Integration](https://github.com/AdeniyiAdesakin/Install-Active-Directory-Domain-Services-and-Join-Client-s-Computer-to-Active-Directory)
- [Group Policy Object Implementations](https://github.com/AdeniyiAdesakin/Group-Policy-Object-GPO-implementations)
- [Windows Server DNS Configuration and Administration](https://github.com/AdeniyiAdesakin/DNS-Configuration)
- [Windows Server DHCP Deployment and Client Lease Validation](https://github.com/AdeniyiAdesakin/DHCP-Installation-and-Configuration)

