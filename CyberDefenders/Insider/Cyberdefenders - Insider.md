# Challenge Report for Insider

- **Challenge Name: [Insider]**
- **Date: [12/19/2021]**
- **URL: [<https://cyberdefenders.org/labs/64>]**
- **Status:[Complete]**

***

## Description

After Karen started working for 'TAAUSAI,' she began to do some illegal activities inside the company. 'TAAUSAI' hired you to kick off an investigation on this case.

You acquired a disk image and found that Karen uses Linux OS on her machine. Analyze the disk image of Karen's computer and answer the provided questions.

## Tools Used

- FTK Imager
- Autopsy

## Analysis

1. What distribution of Linux is being used on this machine?

    **Flag: kali**

    Extracted files and directories using FTK Imager then loaded up in Autopsy.  In Web History noticed "file:///usr/share/kali-defaults/web/homepage.html"

2. What is the MD5 hash of the apache access.log?

    **Flag: d41d8cd98f00b204e9800998ecf8427e**

    ![flag2](/CyberDefenders/Insider/images/Flag2.png)

3. It is believed that a credential dumping tool was downloaded? What is the file name of the download?

    **Flag: mimikatz_trunk.zip**

4. There was a super-secret file created. What is the absolute path?

    **Flag: /root/Desktop/SuperSecretFile.txt**

    ![flag4](/CyberDefenders/Insider/images/Flag4.png)

5. What program used didyouthinkwedmakeiteasy.jpg during execution?

    **Flag: binwalk**

    ![flag5](/CyberDefenders/Insider/images/Flag5.png)

6. What is the third goal from the checklist Karen created?

    **Flag: profit**

    ![flag6](/CyberDefenders/Insider/images/Flag6.png)

7. How many times was apache run?

    **Flag: 0**

    ![flag7](/CyberDefenders/Insider/images/Flag7.png)

    ```text
    Seriously zero?  That's messed up. :)
    ```

8. It is believed this machine was used to attack another. What file proves this?

    **Flag: irZLAohL.jpeg**

    ![flag8](/CyberDefenders/Insider/images/Flag8.png)

9. Within the Documents file path, it is believed that Karen was taunting a fellow computer expert through a bash script. Who was Karen taunting?

    **Flag: Young**

    ![flag9](/CyberDefenders/Insider/images/Flag9.png)

10. A user su'd to root at 11:26 multiple times. Who was it?

    **Flag: postgres**

    ![flag10](/CyberDefenders/Insider/images/Flag10.png)

11. Based on the bash history, what is the current working directory?

    **Flag: /root/documents/myfirsthack/**

    ![flag11](/CyberDefenders/Insider/images/Flag11.png)
