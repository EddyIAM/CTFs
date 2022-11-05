# Challenge Report for BSidesJeddah-Part1

- **Challenge Name: [BSidesJeddah-Part1]**
- **Date: [11/04/2022]**
- **URL: [https://cyberdefenders.org/blueteam-ctf-challenges/81]**
- **Status:[Active]**

***

## Description

As a security consultant, a phishing attack attributed to a popular APT group targeted one of your customers.  Given the provided PCAP trace, analyze the attack and answer challenge questions.

## Tools Used

- BrimSecurity
- suricatarunner
- NetworkMiner
- WireShark
- IDA
- MAC address lookup
- outlook
- VirusTotal
- scdbg
- HxD

## Analysis

1. What is the victim's MAC address?

    **Flag:﹡﹡:﹡﹡:﹡﹡:﹡﹡:﹡﹡:﹡﹡**

2. What is the address of the company associated with the victim's machine MAC address?

    **Flag:﹡﹡﹡﹡ ﹡﹡﹡﹡﹡﹡﹡﹡ ﹡﹡﹡﹡﹡﹡ ﹡﹡﹡﹡ ﹡﹡﹡﹡ ﹡﹡ ﹡﹡﹡﹡﹡ ﹡﹡**

3. What is the attacker's IP address?

    **Flag:﹡﹡﹡.﹡﹡﹡.﹡﹡﹡.﹡﹡﹡**

4. What is the IPv4 address of the DNS server used by the victim machine?

    **Flag:﹡﹡﹡.﹡﹡﹡.﹡﹡﹡.﹡**

5. What domain is the victim looking up in packet 5648?

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡.﹡﹡﹡﹡﹡﹡﹡.﹡﹡﹡﹡﹡﹡.﹡﹡﹡**

6. What is the server certificate public key that was used in TLS session: 731300002437c17bdfa2593dd0e0b28d391e680f764b5db3c4059f7abadbb28e

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡**

7. What domain is the victim connected to in packet 4085?

    **Flag:﹡﹡﹡.﹡﹡﹡﹡﹡﹡-﹡﹡﹡.﹡﹡﹡﹡.﹡﹡﹡﹡﹡﹡﹡﹡﹡.﹡﹡﹡**

8. The attacker conducted a port scan on the victim machine. How many open ports did the attacker find?

    **Flag:﹡**

9. Analyze the pcap using the provided rules. What is the CVE number falsely alerted by Suricata?

    **Flag:﹡﹡﹡-﹡﹡﹡﹡-﹡﹡﹡﹡﹡**

10. What is the command parameter sent by the attacker in packet number 2650?

    **Flag:﹡﹡﹡﹡**

11. What is the stream number which contains email traffic?

    **Flag:﹡﹡﹡﹡**

12. What is the victim's email address?

    **Flag:﹡﹡﹡﹡﹡﹡@﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡.﹡﹡﹡**

13. What was the time attacker sent the email?

    **Flag:﹡﹡:﹡﹡:﹡﹡**

14. What is the version of the program used to send the email?

    **Flag:﹡.﹡﹡**

15. What is the MD5 hash of the email attachment?

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡**

16. What is the CVE number the attacker tried to exploit using the malicious document?

    **Flag:﹡﹡﹡-﹡﹡﹡﹡-﹡﹡﹡﹡﹡**

17. The malicious document file contains a URL to a malicious HTML file. Provide the URL for this file.

    **Flag:﹡﹡﹡﹡://﹡﹡﹡.﹡﹡﹡.﹡﹡﹡.﹡﹡﹡/﹡﹡﹡﹡.﹡﹡﹡﹡**

18. What is the LinkType of the OLEObject related to the relationship which contains the malicious URL?

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡**

19. What is the Microsoft Office version installed on the victim machine?

    **Flag:﹡﹡.﹡.﹡﹡﹡﹡**

20. The malicious HTML contains a js code that points to a malicious CAB file. Provide the URL to the CAB file?

    **Flag:﹡﹡﹡﹡://﹡﹡﹡.﹡﹡﹡.﹡﹡﹡.﹡﹡﹡/﹡﹡﹡﹡.﹡﹡﹡**

21. The exploit takes advantage of a CAB vulnerability. Provide the vulnerability name?

    **Flag:﹡﹡﹡﹡﹡﹡﹡**

22. The CAB file contains a malicious dll file. What is the tool used to generate the dll?

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡**

23. What is the path of the dropped malicious dll file? Replace your username with IEUser

    **Flag:﹡:\﹡﹡﹡﹡﹡\﹡﹡﹡﹡﹡﹡\﹡﹡﹡﹡﹡﹡﹡\﹡﹡﹡﹡﹡\﹡﹡﹡﹡\﹡﹡﹡﹡﹡﹡.﹡﹡﹡**

24. Analyzing the dll file what is the API used to write the shellcode in the process memory?

    **Flag:﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡﹡**

25. Extracting the shellcode from the dll file. What is the name of the library loaded by the shellcode?

    **Flag:﹡﹡﹡﹡﹡﹡﹡**

26. Which port was configured to receive the reverse shell?

    **Flag:﹡﹡﹡**

## Lessons Learned
