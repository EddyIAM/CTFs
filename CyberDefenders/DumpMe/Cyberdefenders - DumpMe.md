# Challenge Report for DumpMe

- **Challenge Name: [DumpMe]**
- **Date: [12/04/2021]**
- **URL: [https://cyberdefenders.org/labs/65]**
- **Status:[Complete]**

***

## Description

Scenario:
One of the SOC analysts took a memory dump from a machine infected with a meterpreter malware. As a Digital Forensicators, your job is to analyze the dump, extract the available indicators of compromise (IOCs) and answer the provided questions.

## Tools Used

Volatility 2

## Analysis

1. What is the SHA1 hash of triage.mem (memory dump)?

    **Flag:c95e8cc8c946f95a109ea8e47a6800de10a27abd**

    ```bash
    $ sha1sum /home/eddy/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem 
    c95e8cc8c946f95a109ea8e47a6800de10a27abd  /home/eddy/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem
    ```

2. What volatility profile is the most appropriate for this machine? (ex: Win10x86_14393)

    **Flag:Win7SP1x64**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem imageinfo
    Volatility Foundation Volatility Framework 2.6.1

    INFO    : volatility.debug    : Determining profile based on KDBG search...
            Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                        AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                        AS Layer2 : FileAddressSpace (/home/eddy/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem)
                        PAE type : No PAE
                            DTB : 0x187000L
                            KDBG : 0xf800029f80a0L
            Number of Processors : 2
        Image Type (Service Pack) : 1
                    KPCR for CPU 0 : 0xfffff800029f9d00L
                    KPCR for CPU 1 : 0xfffff880009ee000L
                KUSER_SHARED_DATA : 0xfffff78000000000L
            Image date and time : 2019-03-22 05:46:00 UTC+0000
        Image local date and time : 2019-03-22 01:46:00 -0400
    $ 
    ```

3. What was the process ID of notepad.exe?

    **Flag:3032**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 pslist | grep notepad.exe
    Volatility Foundation Volatility Framework 2.6.1
    0xfffffa80054f9060 notepad.exe            3032   1432      1       60      1      0 2019-03-22 05:32:22 UTC+0000     
    ```

4. Name the child process of wscript.exe.

    **Flag:UWkpjFjDzM.exe**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 pstree

    Name                                                  Pid   PPid   Thds   Hnds Time
    -------------------------------------------------- ------ ------ ------ ------ ----
      .. 0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000
    ... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000
    $ 
    ```

5. What was the IP address of the machine at the time the RAM dump was created?

    **Flag:10.0.0.101**

    ```bash
    /dev/volatility$ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 netscan
    Volatility Foundation Volatility Framework 2.6.1
    Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created
    0x13e057300        UDPv4    10.0.0.101:55736               *:*                                   2888     svchost.exe    /dev/volatility$ 
    ```

6. Based on the answer regarding the infected PID, can you determine the IP of the attacker?

    **Flag:10.0.0.106**

    ```bash
    /dev/volatility$ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 netscan | grep 3496
    Volatility Foundation Volatility Framework 2.6.1
    0x13e397190        TCPv4    10.0.0.101:49217               10.0.0.106:4444      ESTABLISHED      3496     UWkpjFjDzM.exe 
    /dev/volatility$
    ```

7. How many processes are associated with VCRUNTIME140.dll?

    **Flag:5**

    ```bash
    /dev/volatility$ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 dlllist | grep VCRUNTIME140.dll | wc -l
    Volatility Foundation Volatility Framework 2.6.1
    5
    /dev/volatility$ 
    ```

8. After dumping the infected process, what is its md5 hash?

    **Flag:690ea20bc3bdfb328e23005d9a80c290**

    ```bash
        $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 procdump -D . -p 3496
        Volatility Foundation Volatility Framework 2.6.1
        Process(V)         ImageBase          Name                 Result
        ------------------ ------------------ -------------------- ------
        0xfffffa8005a1d9e0 0x0000000000400000 UWkpjFjDzM.exe       OK: executable.3496.exe
        $ md5sum executable.3496.exe 
        690ea20bc3bdfb328e23005d9a80c290  executable.3496.exe

    ```

9. What is the LM hash of Bob's account?
    **Flag:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0
    **

    ```bash
        $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 hashdump | grep Bob
        Volatility Foundation Volatility Framework 2.6.1
        Bob:1000:aad3b435b51404eeaad3b435b51404ee
        $
    ```

10. What memory protection constants does the VAD node at 0xfffffa800577ba10 have?

    **Flag:PAGE_READONLY**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 vadinfo | grep -A 2 0xfffffa800577ba10
    Volatility Foundation Volatility Framework 2.6.1
    VAD node @ 0xfffffa800577ba10 Start 0x0000000000030000 End 0x0000000000033fff Tag Vad 
    Flags: NoChange: 1, Protection: 1
    Protection: PAGE_READONLY
    ```

11. What memory protection did the VAD starting at 0x00000000033c0000 and ending at 0x00000000033dffff have?

    **Flag:PAGE_NOACCESS**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 vadinfo | grep -A 2 "Start 0x00000000033c0000 End 0x00000000033dffff"
    Volatility Foundation Volatility Framework 2.6.1
    VAD node @ 0xfffffa80052652b0 Start 0x00000000033c0000 End 0x00000000033dffff Tag VadS
    Flags: CommitCharge: 32, PrivateMemory: 1, Protection: 24
    Protection: PAGE_NOACCESS

    ```

12. There was a VBS script that ran on the machine. What is the name of the script? (submit without file extension)

    **Flag:vhjReUDEuumrX**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 cmdline | grep vbs
    Volatility Foundation Volatility Framework 2.6.1
    Command line : "C:\Windows\System32\wscript.exe" //B //NOLOGO %TEMP%\vhjReUDEuumrX.vbs

    ```

13. An application was run at 2019-03-07 23:06:58 UTC. What is the name of the program? (Include extension)

    **Flag:Skype.exe**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 shimcache | grep "2019-03-07 23:06:58 UTC"
    Volatility Foundation Volatility Framework 2.6.1
    2019-03-07 23:06:58 UTC+0000   \??\C:\Program Files (x86)\Microsoft\Skype for Desktop\Skype.exe
    ```

14. What was written in notepad.exe at the time when the memory dump was captured?

    **Flag:flag<REDBULL_IS_LIFE>**

    ```bash
    $ strings -e l ./3032.dmp | grep "flag<"
    flag<REDBULL_IS_LIFE>
    ```

15. What is the short name of the file at file record 59045?

    **Flag:EMPLOY~1.XLS**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 mftparser | grep -A 20 "Record Number: 59045"
    Volatility Foundation Volatility Framework 2.6.1
    Record Number: 59045
    Link count: 2


    $STANDARD_INFORMATION
    Creation                       Modified                       MFT Altered                    Access Date                    Type
    ------------------------------ ------------------------------ ------------------------------ ------------------------------ ----
    2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Archive

    $FILE_NAME
    Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
    ------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
    2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EMPLOY~1.XLS

    $FILE_NAME
    Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
    ------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
    2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EmployeeInformation.xlsx
    ```

16. This box was exploited and is running meterpreter. What was the infected PID?

    **Flag:3496**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/DumpMe/Triage-Memory.mem  --profile=Win7SP1x64 pslist | grep UWkpjFjDzM.exe
    Volatility Foundation Volatility Framework 2.6.1
    0xfffffa8005a1d9e0 UWkpjFjDzM.exe         3496   5116      5      109      1      1 2019-03-22 05:35:33 UTC+0000                                 

    ```
