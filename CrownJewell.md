Sherlock Scenario
Forela's domain controller is under attack. The Domain Administrator account is believed to be compromised, and it is suspected that the threat actor dumped the NTDS.dit database on the DC. We just received an alert of vssadmin being used on the DC, since this is not part of the routine schedule we have good reason to believe that the attacker abused this LOLBIN utility to get the Domain environment's crown jewel. Perform some analysis on provided artifacts for a quick triage and if possible kick the attacker as early as possible.

CrownJewel1.zip
10 MB

Task 1
Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.

2024-05-14 03:42:16

A simple googling, help us come accross event code 7036, that is generated when a service state changes e.g start or stop. We then filter the system logs by this event and go through them one by one. We get the event below. Opening the the details tab and in the xml format we get the date.
![Screenshot (1599)](https://github.com/user-attachments/assets/6fdfbff2-e7c7-4ebc-ac5e-14925261f15d)
![Screenshot (1600)](https://github.com/user-attachments/assets/f9f08dd4-ea2d-4b8e-8b97-e70fd78c4101)

Task 2
When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the User groups it enumerates, the Subject Account name, and also identify the Process ID(in decimal) of the Volume shadow copy service process

Administrators, Backup Operators, DC01$

Going through the Security event logs, I notice event code 4799 that enumerates group permissions. To narrow down the focus, we are looking for vssvc.exe that manages and implements Volume Shadow Copies used for backup and other purposes. We get this two groups under the account name DC01$. Just remember to add the spaces.
![Screenshot (1602)](https://github.com/user-attachments/assets/9efaf2d1-91c4-45f7-a039-23eb09ae0118)
![Screenshot (1603)](https://github.com/user-attachments/assets/119bbbef-0d7e-493d-8c9a-936e7da9386e)

Task 3
Identify the Process ID (in Decimal) of the volume shadow copy service process.

4496

From the above event, we get the process id 0x1190, We do a conversion and get 4496.
![Screenshot (1604)](https://github.com/user-attachments/assets/db328a43-a429-46c3-87d4-b553059c6791)
![image](https://github.com/user-attachments/assets/643d30c8-689f-4a2f-9cd2-3a51e07c651b)

Task 4
Find the assigned Volume ID/GUID value to the Shadow copy snapshot when it was mounted.

{06c4a997-cca8-11ed-a90f-000c295644f9}

Going through Microsoft windows NTFS logs, I see volume mount event id 4. I also notice that it is a shadow copy snapshot. 
![Screenshot (1605)](https://github.com/user-attachments/assets/68acf966-052d-4d97-a17c-f50c72af0514)

Task 5
Identify the full path of the dumped NTDS database on disk.

C:\Users\Administrator\Documents\backup_sync_dc\ntds.dit

For this prompt, I opend the $MFT file using the MFTEplorer, one of Eric Zimmerman tools for forensics. Normally this kind of data is found under users accounts, Investigating the accounts, I noticed Administrator account. I checked this account and found the copied ntds file in the path C:\Users\Administrator\Documents\backup_sync_dc\ntds.dit
![Screenshot (1606)](https://github.com/user-attachments/assets/30f2bd4c-d7e7-4dd7-8c3f-ad05c9cb1920)

Task 6
When was newly dumped ntds.dit created on disk?

2024-05-14 03:44:22

I checked the " Is Created On" tab of the same $MFT file and found the time above.
![Screenshot (1607)](https://github.com/user-attachments/assets/787fa435-2fb2-4587-bea0-acdc8f755137)


Task 7
A registry hive was also dumped alongside the NTDS database. Which registry hive was dumped and what is its file size in bytes?

SYSTEM, 17563648

On the same folder path, there is another file with the name SYSTEM, Checked the physical size which is given in hex format. Since the question is asking for the size in bytes, we need to convert it to decimal. We end up with 17563648. 
![Screenshot (1608)](https://github.com/user-attachments/assets/24e6fe47-2a68-4f1e-bf87-8db8a0e910ff)
![image](https://github.com/user-attachments/assets/548d985f-c78a-44ef-9ea6-64f57acec374)


