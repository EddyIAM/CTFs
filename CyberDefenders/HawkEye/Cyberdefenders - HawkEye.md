# Challenge Report for HawkEye

- **Challenge Name: [HawkEye]**
- **Date: [11/05/2022]**
- **URL: [https://cyberdefenders.org/blueteam-ctf-challenges/91]**
- **Status:[Complete]**

***

## Description

An accountant at your organization received an email regarding an invoice with a download link. Suspicious network traffic was observed shortly after opening the email. As a SOC analyst, investigate the network trace and analyze exfiltration attempts.

## Tools Used

- Wireshark
- BrimSecurity
- Apackets
- MaxMind Geo IP
- VirusTotal

## Analysis

1. How many packets does the capture have?

    **Flag:4003**

    In Wireshark go to Statistics -> Capture file properties

2. At what time was the first packet captured?

    **Flag: 2019-04-10 20:37:07 UTC**

    In Wireshark go to Statistics -> Capture file properties -> Time -> First Packet and covert to UTC.

3. What is the duration of the capture?

    **Flag: 01:03:41**

    In Wireshark go to Statistics -> Capture file properties -> Time -> Elapsed.

4. What is the most active computer at the link level?

    **Flag: 00:08:02:1c:47:ae**

    In Wireshark go to Statistics -> Conversations -> Ethernet

5. Manufacturer of the NIC of the most active system at the link level?

    **Flag:Hewlett-Packard**

    In Wireshark go to Statistics -> Conversations -> Ethernet and check Name Resolution.  That gets you HewlettP - Google it.

6. Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?

    **Flag:Palo Alto**

    Google Hewlett Packard HQ

7. The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?

    **Flag:3**

    Look at Statistics -> Conversations -> IPv4.  Find the private addresses.  Count them up and remove the .255.

8. What is the name of the most active computer at the network level?

    **Flag: Beijing-5cd1-PC**

    Filter packets on DHCP.  Find the DHCP inform and look at DHCP option 12.

9. What is the IP of the organization's DNS server?

    **Flag: 10.4.10.4**

    Filter on DNS and see who all the requests are going to.

10. What domain is the victim asking about in packet 204?

    **Flag: proforma-invoices.com**

    Go to packet 204 and look at the packet info on the right.

11. What is the IP of the domain in the previous question?

    **Flag: 217.182.138.150**

    dns.qry.name == proforma-invoices.com and look at response.

12. Indicate the country to which the IP in the previous section belongs.

    **Flag:france**

    ```bash
    $whois 217.182.138.150
    ```

13. What operating system does the victim's computer run?

    **Flag: Windows NT 6.1**

    Filter for http and look at the user agent.

14. What is the name of the malicious file downloaded by the accountant?

    **Flag:  tkraw_Protected99.exe**

    Found under Analyze -> Expert Information

15. What is the md5 hash of the downloaded file?

    **Flag:71826ba081e303866ce2a2534491a2f7**

    File -> Export Objects -> HTTP -> Select and save tkraw-Protected99.exe.  Then run through HashMyFiles.

16. What is the name of the malware according to Malwarebytes?

    **Flag: Spyware.HawkEyeKeyLogger**

    Use Virus Total to find the name.

17. What software runs the webserver that hosts the malware?

    **Flag: LiteSpeed**

    Follow the HTTP stream that did the download and look for a HTTP/1.1 200 OK

    ```text
    HTTP/1.1 200 OK
    Last-Modified: Wed, 10 Apr 2019 04:44:31 GMT
    Content-Type: application/x-msdownload
    Content-Length: 2025472
    Accept-Ranges: bytes
    Date: Wed, 10 Apr 2019 20:37:54 GMT
    Server: LiteSpeed
    Connection: Keep-Alive
    ```

18. What is the public IP of the victim's computer?

    **Flag: 173.66.146.112**

    You can find it in the SMTP server hello section.

    ```text
    250-p3plcpnl0413.prod.phx3.secureserver.net Hello Beijing-5cd1-PC [173.66.146.112]
    ```

19. In which country is the email server to which the stolen information is sent?

    **Flag: United States**

    Filter for SMTP and find the server IP (23.229.162.69) then Google whois ip 23.229.162.69`

20. What is the domain's creation date to which the information is exfiltrated?

    **Flag:2014-02-08**

    In email we find an email being sent to/from sales.del@macwinlogistics.in so we do a whois macwinlogistics.in.

21. Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?

    **Flag: Exim 4.91**

    Follow the SMTP conversation and look at the beginning.

    ```text
    220-p3plcpnl0413.prod.phx3.secureserver.net ESMTP Exim 4.91 #1 Wed, 10 Apr 2019 13:38:15 -0700 
    220-We do not authorize the use of this system to transport unsolicited, 
    220 and/or bulk e-mail.
    ```

22. To which email account is the stolen information sent?

    **Flag: sales.del@macwinlogistics.in**

    We ran across this in question #20.

23. What is the password used by the malware to send the email?

    **Flag: Sales@23**

    In the same conversation we followed in question 21  find the auth section.  

    ```text
    AUTH login c2FsZXMuZGVsQG1hY3dpbmxvZ2lzdGljcy5pbg==
    334 UGFzc3dvcmQ6
    U2FsZXNAMjM=
    ```

    The password is base64 encoded so run it through base64 to get the answer.

    ```bash
    $ echo 'U2FsZXNAMjM=' |  base64 -d
    Sales@23
    ```

24. Which malware variant exfiltrated the data?

    **Flag: Reborn V9**

    In looking at the email in the conversation we are following we notice the subject is encoded base 64.

    ```text
    From: sales.del@macwinlogistics.in
    To: sales.del@macwinlogistics.in
    Date: 10 Apr 2019 20:38:08 +0000
    Subject: =?utf-8?B?SGF3a0V5ZSBLZXlsb2dnZXIgLSBSZWJvcm4gdjkgLSBQYXNzd29yZHMgTG9ncyAtIHJvbWFuLm1jZ3VpcmUgXCBCRUlKSU5HLTVDRDEtUEMgLSAxNzMuNjYuMTQ2LjExMg==?=
    Content-Type: text/plain; charset=utf-8
    ```

    If we decode it we get...

    ```bash
    $ echo 'SGF3a0V5ZSBLZXlsb2dnZXIgLSBSZWJvcm4gdjkgLSBQYXNzd29yZHMgTG9ncyAtIHJvbWFuLm1jZ3VpcmUgXCBCRUlKSU5HLTVDRDEtUEMgLSAxNzMuNjYuMTQ2LjExMg==?=' |  base64 -d
    HawkEye Keylogger - Reborn v9 - Passwords Logs - roman.mcguire \ BEIJING-5CD1-PC - 173.66.146.112
    ```

25. What are the bankofamerica access credentials? (username:password)

    **Flag: roman.mcguire:P@ssw0rd$**

    The content of the email is also base64 encoded.  It's long so run through [Cyber Chef](https://gchq.github.io/CyberChef/).

26. Every how many minutes does the collected data get exfiltrated?

    **Flag: 10**

    If you filter for the EHLO command (smtp.req.command == "EHLO") you can see it happens about every 10 minutes.

## Lessons Learned
