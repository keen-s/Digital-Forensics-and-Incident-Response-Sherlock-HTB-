In this Sherlock, you will become acquainted with MFT (Master File Table) forensics. You will be introduced to well-known tools and methodologies for analyzing MFT artifacts to identify malicious activity. During our analysis, you will utilize the MFTECmd tool to parse the provided MFT file, TimeLine Explorer to open and analyze the results from the parsed MFT, and a Hex editor to recover file contents from the MFT.
BFT.zip


Task 1

Simon Stark was targeted by attackers on February 13. He downloaded a ZIP file from a link received in an email. What was the name of the ZIP file he downloaded from the link?

Stage-20240213T093324Z-001.zip
In this request we first convert the $MFT file to a .csv file using the ".\MFTECmd.exe -f 'sourc\file\$MFT' --csv 'destination\folder\' --csvf MFT.csv'. After this, we open the .cvs file in timeline explorer. Since we are given the time that the download happen and that the file was a zip file, we filter for these in the "Created0x10" and "Extension" respectively. We see two .zip files Kape and Stage-20240213T093324Z-001.zip. We take the second one as its more suspicious and from the same screen, we can see that this file was used to open other suspicious .zip files.
![Screenshot (1628)](https://github.com/user-attachments/assets/10d76492-14bd-4e7d-a83b-948a8586d2f4)


Task 2

Examine the Zone Identifier contents for the initially downloaded ZIP file. This field reveals the HostUrl from where the file was downloaded, serving as a valuable Indicator of Compromise (IOC) in our investigation/analysis. What is the full Host URL from where this ZIP file was downloaded?

[https://xxxxxx](https://storage.googleapis.com/drive-bulk-export-anonymous/20240213T093324.039Z/4133399871716478688/a40aecd0-1cf3-4f88-b55a-e188d5c1c04f/1/c277a8b4-afa9-4d34-b8ca-e1eb5e5f983c?authuser)
Here, we are looking for the zone Identifier. using our zip extention, we add the zone identifier ( Stage-20240213T093324Z-001.zip:Zone.Identifier ) and search using the find menu we see the link that the user accessed.
![Screenshot (1629)](https://github.com/user-attachments/assets/8f49ddf2-0d68-4b61-8a49-362192bd9f42)


Task 3

What is the full path and name of the malicious file that executed malicious code and connected to a C2 server?

C:\Users\simon.stark\Downloads\Stage-20240213T093324Z-001\Stage\invoice\invoices\invoice.bat
Here we search for "Stage-20240213T093324Z-001" using the find function to try and trace paths where this folder name was used. We are trying to see any suspicious files. We find invoice.bat.
![Screenshot (1630)](https://github.com/user-attachments/assets/eb7a9734-75b0-4281-9ea6-ea9d43c50a94)


Task 4

Analyze the $Created0x30 timestamp for the previously identified file. When was this file created on disk?

2024-02-13 16:38:39
Here since we have the suspicous file, we just check the time on when it was created on disk as instructed.
![Screenshot (1631)](https://github.com/user-attachments/assets/08482624-7cb2-4910-8821-32265d08ad51)


Task 5

Finding the hex offset of an MFT record is beneficial in many investigative scenarios. Find the hex offset of the stager file from Question 3.

16E3000
Here we followed the hint provided, we get the entry number and multiply with 1024 then convert to hexadecimal value
![Screenshot (1632)](https://github.com/user-attachments/assets/ed59609e-7654-4175-b829-eb2ba1ab4f54)

![image](https://github.com/user-attachments/assets/29ed78e3-3bc8-4c21-a8a9-52c75049d742)

Task 6

Each MFT record is 1024 bytes in size. If a file on disk has smaller size than 1024 bytes, they can be stored directly on MFT File itself. These are called MFT Resident files. During Windows File system Investigation, its crucial to look for any malicious/suspicious files that may be resident in MFT. This way we can find contents of malicious files/scripts. Find the contents of The malicious stager identified in Question3 and answer with the C2 IP and port.

43.204.110.203:6666
In MFT Explorer, we search for invoice.bat location. At the bottom, we find that the file data was also recorded
![Screenshot (1633)](https://github.com/user-attachments/assets/eaf38740-5a0c-4e67-a958-640fe97ff0f3)


This CTF helps learners practice using Eric Zimmerman tools (in this case the Timeline Explorer and MFT Explorer) to do digital forencis and incident response. The highlight, was learning how to get the hex offset using timeExplorer, Initially I had tried the offset values available in the MFT Explorer but none worked. This is a new knowledge point that I will add to my internal hardware going forward.
