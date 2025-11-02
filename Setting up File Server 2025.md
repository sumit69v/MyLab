## Setting up File server on Windows server 2025

A file server is the core of any windows based network, providing centralized storage, access control and sharing capabilities for users and devices across the domain. 

With this steps you can setup the windows server with
- file server
- configure sharing and providing Sharing and NTFS permission

A properly configured file server allows you to 
- Centralized data storage
- Simplify backups and disaster recovery
- Enforce NTFS and share permission
- Monitor and audit access
- Integrated with Active Directory

# Steps
1. Install File server role
- open server manager > click manage > Add Roles and Features
- Next > Features based installation > Next
- Select your domain target
- Expand File and Storage Services > File and iSCSI services
- Check File server and click Next (If it is already checked, it showed marked as Installed)
- Click Install
- After completion close to return the server manager

2. Create a share folder
- Server manager > File and Storage Services > Shares
- Tasks > New Share > SMB Share - Quick (Recommended)
- Select the volume or path where the folder will be hosted e.g. \\DC1\Shares\TeachersApps
- Enter the share name and share discription
- Review share settings - enable Encrypt data access if you want to use SMB encryption > Next
- Configure permission
- Click Customize permissions
- Select Advance > Advance Security Settings windows
- Disable inheritance
- Convert inherited permissions into explicit permissions
Remove any unnecessary entries (such as "everyone" or "users") to prevent unrestriceted access. Ensure only the following permissions remain
- Administrators -> Full control
- System -> Full control
- Creator Owner -> Full control (subfolders and files only)
- check - Replace all child object permission entires with inheritable permission entries from this object. 

This configuration ensures your IT administrator maintain full control while keeping the share locked down until you create department-level security groups later.

Once your domain groups are ready, you can return here to grant role based access such as 
- Domain Admin --> Full Control
- Dept_Groups (eg. Staffs, HR) --> Modify
- Domain users --> Read/Execute (Optional)

- Click OK, Then next to move the confirmation step. 


# Configuring NTFS and Share Permissions
Once your shared folder is created, its important to configure NTFS and Share Permissions correctly. These two layers work together to define who can access, modify or manage files on your file server. 

Getting this right to ensure security, complicance and clarity, espacially when working in multi-user or department environments
1. understanding NTFS and Share Permissions
- Shared Permission apply when accessing files over the network (eg. \\dc1\shares\teachersapp)
- NTFS permissions apply both locally and over the network, and are more granular
- when both applies, the most restrictive permission wins

Example: If a user has Full Control on the Share level but only Read on NTFS, the effective permission is Read. 

2. Best practice : set broad share permissions and restrict with NTFS
- Set share permissions to Everyone : Full control 
- Use NTFS Permission to manage specific access

This approach simplify troubleshooting while maintaining precise access control

3. Configure NTFS permission 
- Right click to your shared folder -> Properties -> Security tab -> Advance
- Remove any unwanted inherited permission that might give broad access
- click add toassign groups or users from Active Directory
Define Access level
- Read & Execute - For general users who only need to open and view files
- Modify - For user groups that need to edit or create files
- Full Control - reserver for the IT administrators or folder owners
Enable "Replace all child object permissions" if you want the same permissions applied to all subfolders.

4. Verify access from a client machine


# Mapping Network drives via Group policy
Once the file share folder and permissions are configured. Rather than manually mapping drives on every worksttion, you can automate the process using group policy preferences in the server.
1. Open Group Policy Management Console
2. Expand your forest -> Domain -> Your Domain
3. Create or Edit a Group Policy Object (GPO)
4. Right click your domain or target OU and choose Create a GPO in this domain, and link it here...
5. Name the policy
6. Right click policy and Edit
7. Configure Drive Mapping
- User Configuration -> Preferences -> Windows Settings -> Drive Maps
- Right click -> New -> Mapped Drive

Configure following steps
- Location: \\dc1\shares\TeachersApps
- Drive letter : Choose
- Label as : Name it
- Reconnect : Check (ensures persistance after reboot)

Uder the Common Tab, we can set Item-level targeting to apply the mapping only to specific users or groups once they are created.

8. Testing and Verification
- Update group policy on the client
	gpupdate /force
- Reboot or sign out
- Relogin to verify drive.

9. Test access permissions

