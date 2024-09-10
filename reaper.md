Sherlock Scenario
Our SIEM alerted us to a suspicious logon event which needs to be looked at immediately . The alert details were that the IP Address and the Source Workstation name were a mismatch .You are provided a network capture and event logs from the surrounding time around the incident timeframe. Corelate the given evidence and report back to your SOC Manager.

Task 1
What is the IP Address for Forela-Wkstn001?
172.17.79.129
From the pcap file, while checking the details of the first event, the hostname is revealed to be Forela-Wkstn001, we can see the source ip also.
![Pasted image 20240828040224](https://github.com/user-attachments/assets/4f65d1d4-5bad-4352-8dea-cb9cf3a18b69)

Task 2
What is the IP Address for Forela-Wkstn002?
172.17.79.136

Going through the pcap file in wireshark, we discover the use of NBNS procal. This protocol is used for internal name resolution when the name is not identified in the DNS. Checking the details, we see the host name Forela-Wkstn002. We get its ip address.
![Pasted image 20240828042422](https://github.com/user-attachments/assets/da38c71f-c364-4f3d-92d2-339b93cf6bba)


Task 3
Which user account's hash was stolen by attacker?
arthur kyle

I searched for event code 4624 which indicates successful logon. From here, I was looking for suspicious event that has conflicting user information. Lucky for me I saw the ip address that did not match the work station name. Also remember to separate the name into two distinct names.
![Pasted image 20240828042422](https://github.com/user-attachments/assets/8b0c761b-ad20-4daa-aa17-07378c3f9ac3)


Task 4
What is the IP Address of Unknown Device used by the attacker to intercept credentials?
172.17.79.135

While looking at arp requests and responses, I noticed that this ip address  prompted for multiple ip addresses requesting to know who owns the ip addresses. These ip addresses were in consecutive order and in a very short time. I checked the NBNS protocal and noticed that this ip address does not have any account name registered. 
![Pasted image 20240828045319](https://github.com/user-attachments/assets/3acc47ba-4fd9-47f1-a152-602b9915de55)

Task 5
What was the fileshare navigated by the victim user account?
\\DC01\Trip
By filtering for smb2 and the victim's ip address I get the share that the victim navigated to.
![[Pasted image 20240828064202.png]]

Task 6
What is the source port used to logon to target workstation using the compromised account?
40252
![Screenshot (2)](https://github.com/user-attachments/assets/563e027d-461d-43e0-b656-9be3460865fa)


Task 7
What is the Logon ID for the malicious session?
0x64a799
![Screenshot (1)](https://github.com/user-attachments/assets/0f94afbd-28e9-4f52-b078-6563298124f1)


Task 8
The detection was based on the mismatch of hostname and the assigned IP Address.What is the workstation name and the source IP Address from which the malicious logon occur?
FORELA-WKSTN002, 172.17.79.135

From the same event as above, you can find these details.
![Pasted image 20240828060442](https://github.com/user-attachments/assets/a2962b50-40ad-4962-ad8a-20998368c59a)


Task 9
When did the malicious logon happened. Please make sure the timestamp is in UTC?
2024-07-31 04:55:16
![Screenshot (4)](https://github.com/user-attachments/assets/65011f8e-d855-43c9-93fa-a87a3ec8f74e)


Task 10
What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?
\\*\IPC$

I looked for event 5140 which shows if a network share object has been accessed, I only find one event. This event has the share
![Pasted image 20240828061159](https://github.com/user-attachments/assets/a2f7e3d8-e6d1-41bc-9d11-8a90905c314f)



Generally this particular Sherlock helps sharpen event logs and wireshark analysis skills. Enables a person to practice this fundamental principals.

