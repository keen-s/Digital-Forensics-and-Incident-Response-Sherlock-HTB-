Sherlock Scenario
Forela's Domain environment is pure chaos. Just got another alert from the Domain controller of NTDS.dit database being exfiltrated. Just one day prior you responded to an alert on the same domain controller where an attacker dumped NTDS.dit via vssadmin utility. However, you managed to delete the dumped files kick the attacker out of the DC, and restore a clean snapshot. Now they again managed to access DC with a domain admin account with their persistent access in the environment. This time they are abusing ntdsutil to dump the database. Help Forela in these chaotic times!!
CrownJewel2.zip


Task 1
When utilizing ntdsutil.exe to dump NTDS on disk, it simultaneously employs the Microsoft Shadow Copy Service. What is the most recent timestamp at which this service entered the running state, signifying the possible initiation of the NTDS dumping process?

YYYY-MM-DD HH:MM:SS

Task 2
Identify the full path of the dumped NTDS file.
C:\Windows\Temp\dump_tmp\Active Directory\ntds.dit

Here we filter for event id 325 (TaskScheduler) for creating new database
![Screenshot (1610)](https://github.com/user-attachments/assets/5dc1355d-0461-4d51-b6e7-0de49de948e8)

Task 3
When was the database dump created on the disk?
2024-05-15 05:39:55

Here I opened the system logs and did a simple "find" for shadow copy, the first event was "shadow copy started".
![Screenshot (1609)](https://github.com/user-attachments/assets/e1742524-5d45-47c1-91cc-8487c0c0e123)

Task 4

When was the newly dumped database considered complete and ready for use?

2024-05-15T05:39:58

Here we look for event code 327 that show that the database engine detached a database meaning that this database is ready for use.
![Screenshot (1611)](https://github.com/user-attachments/assets/cd166684-a8b4-4bc0-8e04-f8759341cb08)



Task 5
Event logs use event sources to track events coming from different sources. Which event source provides database status data like creation and detachment?

ESENT
From our previous questions, we check the source field of the events in question
![Screenshot (1612)](https://github.com/user-attachments/assets/59446f49-c466-4218-894c-f2a312d82059)


Task 6
When ntdsutil.exe is used to dump the database, it enumerates certain user groups to validate the privileges of the account being used. Which two groups are enumerated by the ntdsutil.exe process? Also, find the Logon ID so we can easily track the malicious session in our hunt.

Administrators, Backup Operators, 0x8DE3D

For this event, I opened the security logs and used the find function to find any instance where the process ntdsutil.exe was used. I found these two groups.

![Screenshot (1613)](https://github.com/user-attachments/assets/341d1c11-7156-4e32-90ba-7d946a3a210d)
![Screenshot (1614)](https://github.com/user-attachments/assets/fb4ab88d-c2fd-47fb-a262-777eb0f3eb02)
![image](https://github.com/user-attachments/assets/a0e622dc-8540-461a-b8c6-dc0455424ae3)


Task 7
Now you are tasked to find the Login Time for the malicious Session. Using the Logon ID, find the Time when the user logon session started.

2024-05-15 05:36:31
For this task we look for event code 5379 that is logged when a user performs a read operation on stored credentials in Credential Manager. We then use the find function to locate the logon ID, We also make sure that its a user account and not service account (should not end with '$' ). Now check the time.
![Screenshot (1617)](https://github.com/user-attachments/assets/de5df9fa-b7e6-415f-a8cb-bba0f2b69329)

This CTF helps participants have a holistic view of this particular attack. Enabling users to practice and learn how processes affect other processes and how to connect or trace the attackers activities. Its good practice when trying to understand windows event logs.
