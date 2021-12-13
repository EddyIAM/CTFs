# Challenge Report for EscapeRoom

- **Challenge Name: EscapeRoom**
- **Date: Done before I started documenting.**
- **URL: <https://cyberdefenders.org/labs/18>**
- **Status: Complete**

***

## Description

You belong to a company specializing in hosting web applications through KVM-based Virtual Machines. Over the weekend, one VM went down, and the site administrators fear this might be the result of malicious activity. They extracted a few logs from the environment in hopes that you might be able to determine what happened.
This challenge is a combination of several entry to intermediate-level tasks of increasing difficulty focusing on authentication, information hiding, and cryptography. Participants will benefit from entry-level knowledge in these fields, as well as knowledge of general Linux operations, kernel modules, a scripting language, and reverse engineering. Not everything may be as it seems. Innocuous files may turn out to be malicious so take precautions when dealing with any files from this challenge.

## Tools Used

- VS Code
- WireShark
- UPX
- Ghidra

## Analysis

1. What service did the attacker use to gain access to the system?

    **SSH**

2. What attack type was used to gain access to the system?(one word)

    **bruteforce**

    ```text
    Use the ssh filter on wireshark to see all the failed attempts
    ```

3. What was the tool the attacker possibly used to perform this attack?

    **hydra**

4. How many failed attempts were there?

    **52**

5. What credentials (username:password) were used to gain access? Refer to shadow.log and sudoers.log.

    **manager:forgot**

6. What other credentials (username:password) could have been used to gain access also have SUDO privileges? Refer to shadow.log and sudoers.log.

    **sean:spectre**

7. What is the tool used to download malicious files on the system?

    **wget**

8. How many files the attacker download to perform malware installation?

    **3**

9. What is the main malware MD5 hash?

    **772b620736b760c1d736b1e6ba2f885b**

    ```text
    Using Wireshark do a file -> Export Objects -> Http and export the files.  Then run md4sum to get the hash.
    ```

10. What file has the script modified so the malware will start upon reboot?

    **/etc/rc.local**

    ```text
    From the install script; echo -e "/var/mail/mail &\nsleep 1\npidof mail > /proc/dmesg\nexit 0" > /etc/rc.local
    ```

11. Where did the malware keep local files?

    **/var/mail/**

    ```text
    From the install script; echo -e "/var/mail/mail &\nsleep 1\npidof mail > /proc/dmesg\nexit 0" > /etc/rc.local
    ```

12. What is missing from ps.log?

    **/var/mail/mail**

13. What is the main file that used to remove this information from ps.log?

    **sysmod.ko**

    ```text
    mv 2 /lib/modules/`uname -r`/sysmod.ko
    depmod -a
    echo "sysmod" >> /etc/modules
    modprobe sysmod
    ```

14. Inside the Main function, what is the function that causes requests to those servers?

    **requestFile**

15. One of the IP's the malware contacted starts with 17. Provide the full IP.

    **174.129.57.253**

    ```text
    Wireshark -> Statistics -> Conversations -> 1PV4
    ```

16. How many files the malware requested from external servers?

    **9**

17. What are the commands that the malware was receiving from attacker servers? Format: comma-separated in alphabetical order

    **NOP,RUN**

    ```text
    This was fun.  Take the malware, unpack it, then run it thru Ghidra.  The Main function points to another function that does a parameter check for the commands.  Use CyberChef to change from hex to ascii.
    ```
