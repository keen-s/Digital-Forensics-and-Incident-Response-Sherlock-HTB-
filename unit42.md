Sherlock Scenario
In this Sherlock, you will familiarize yourself with Sysmon logs and various useful EventIDs for identifying and analyzing malicious activities on a Windows system. Palo Alto's Unit42 recently conducted research on an UltraVNC campaign, wherein attackers utilized a backdoored version of UltraVNC to maintain access to systems. This lab is inspired by that campaign and guides participants through the initial access stage of the campaign.
unit42.zip


Task 1

How many Event logs are there with Event ID 11?

56
Filter for the events and get the number of events
![Screenshot (1618)](https://github.com/user-attachments/assets/6b059c5b-3f97-47de-8d4d-1ea048ea8e43)


Task 2

Whenever a process is created in memory, an event with Event ID 1 is recorded with details such as command line, hashes, process path, parent process path, etc. This information is very useful for an analyst because it allows us to see all programs executed on a system, which means we can spot any malicious processes being executed. What is the malicious process that infected the victim's system?

C:\Users\CyberJunkie\Downloads\Preventivo24.02.14.exe.exe
In just six events, its easy to see an executable in the downloads forlder. 
![Screenshot (1619)](https://github.com/user-attachments/assets/7ca831c6-be0f-4d08-bf6b-88a9916a9150)


Task 3

Which Cloud drive was used to distribute the malware?

dropbox
Here we look for DNS connection. We notice that the process firefox.exe opened dropbox.com, during the same timeframe, the file Preventivo24.02.14.exe.exe is created in the downloads folder. Later, this file is trying to access example.com, possibly to confirm internet connectivity
![Screenshot (1620)](https://github.com/user-attachments/assets/25a37da8-0dc8-4372-bd01-aa19060fdaba)

Task 4

The initial malicious file time-stamped (a defense evasion technique, where the file creation date is changed to make it appear old) many files it created on disk. What was the timestamp changed to for a PDF file?

2024-01-14 08:10:06
Here we look for event code 2 (A process changed a file creation time) to try and detect the timestomp activity, then we use the find function to search for .pdf
![Screenshot (1621)](https://github.com/user-attachments/assets/10a00cbc-c310-45d1-bba8-e289ace5f49f)


Task 5

The malicious file dropped a few files on disk. Where was "once.cmd" created on disk? Please answer with the full path along with the filename.

 C:\Users\CyberJunkie\AppData\Roaming\Photo and Fax Vn\Photo and vn 1.1.2\install\F97891C\WindowsVolume\Games\once.cmd
Here, we filter for file create event code 11, we then use the find function to locate our malicious image file Preventivo24.02.14.exe.exe, we get the path
![Screenshot (1627)](https://github.com/user-attachments/assets/ee34336c-32f5-4c06-a8f3-3cb31c1bd99b)



Task 6

The malicious file attempted to reach a dummy domain, most likely to check the internet connection status. What domain name did it try to connect to?

www.example.com
Here we check for any DNS connections.
![Screenshot (1624)](https://github.com/user-attachments/assets/bc73552d-0610-44c9-8851-887cda5907c7)



Task 7

Which IP address did the malicious process try to reach out to?

93.184.216.34
Here, We look for any network connection event code 3. We only find one event associated with our malicious file, it accesses the ip address on port 80
![Screenshot (1625)](https://github.com/user-attachments/assets/6e0a56bb-f390-4fd7-991e-3260477a65f0)


Task 8

The malicious process terminated itself after infecting the PC with a backdoored variant of UltraVNC. When did the process terminate itself?

2024-02-14 03:41:58
Here, we look for event code 5 Process terminated, normally we should filter for our malicious image but since we find only one event that should still do. 
![Screenshot (1626)](https://github.com/user-attachments/assets/2c1c40d6-4f71-4440-a2cf-ee0ae615c52b)
