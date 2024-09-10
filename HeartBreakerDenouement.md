Sherlock Scenario
Your digital forensics expertise is critical to determine whether data exfiltration has occurred from the customer’s environment. Initial findings include a compromised AWS credential, indicating a potential unauthorized access. This investigation follows from a customer report of leaked data allegedly for sale on the darknet market. By examining the compromised server and analyzing the provided AWS logs, it will not only validate the customer's breach report but also provide valuable insights into the attacker's tactics, enabling a more comprehensive response.

HeartBreakerDenouement.zip


**Task 1**
**What type of scanning technique was used to discover the web path of the victim's web server? Specify the name of the corresponding MITRE sub-technique.**

Wordlist Scanning

This are  /root/var/log/apache2 access.logs from the root directory.
From the screenshot, you will notice the paths being searched for are not random. They follow a pattern trying to use commonly used website paths.

![Pasted image 20240824125700](https://github.com/user-attachments/assets/d2e377f7-fca4-41a4-ad65-9e447772204c)

When we check for the type of scanning where brute force attack is used. We find Worldlist Scanning.
![Pasted image 20240824125959](https://github.com/user-attachments/assets/865ed342-d1e1-4f15-8e47-d710911810a6)


**Task 2
It seems a web request possibly could have been rerouted, potentially revealing the web server's web path to the Threat Actor. What specific HTML status code might have provided this information?**

301
While going through /root/var/log/apache2 access logs, I came across this GET request error code which is usually related to 
![Pasted image 20240824123644](https://github.com/user-attachments/assets/b3516197-2ae0-4350-b359-93a5351f6f36)


**Task 3
What was the initial payload submitted by the threat actor to exploit weakness of the web server?**


**Task 4
What is the name of the vulnerability exploited by the Threat Actor?**

Avoid punctuation for clear formatting.

**Task 5
At what time (UTC) did the Threat Actor first realize they could access the cloud metadata of the web server instance?**

YYYY-MM-DD hh:mm:ss

**Task 6
For a clearer insight into the Database content that could have been exposed, could you provide the name of at least one of its possible tables?**


**Task 7
Which AWS API call functions similarly to the 'whoami' command in Windows or Linux?
GetCallerIdentity**

**Task 8
It seems that the reported compromised AWS IAM credential has been exploited by the Threat Actor. Can you identify the regions where these credentials were used successfully? Separate regions by comma and in ascending order.**


**Task 9
Discovering that the compromised IAM account was used prior to the web server attack, this suggests the threat actor might have obtained the public IP addresses of running instances. Could you specify the API call the could have exposed this information?**


**Task 10
Looks like the Threat Actor didn’t only use a single IP address. What is the total number of unsuccessful requests made by the Threat Actor?
**
Integer, e.g - 65

**Task 11
Can you identify the Amazon Resource Names (ARNs) associated with successful API calls that might have revealed details about the victim's cloud infrastructure? Separate ARNs by comma and in ascending order.**
arn,arn

**Task 12
Evidence suggests another database was targeted. Identify all snapshot names created. Separate names by comma and in ascending order.**


**Task 13
The Threat Actor successfully exfiltrated the data to their account. Could you specify the account ID that was used?
**

**Task 14
Which MITRE Technique ID corresponds to the activity described in Question 13?**
T1537
![Pasted image 20240827010553](https://github.com/user-attachments/assets/44880861-83ea-4f71-9cb2-a67215e1ccbc)
