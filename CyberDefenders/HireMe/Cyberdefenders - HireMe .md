# Challenge Report for HireMe

- **Challenge Name: [HireMe]**
- **Date: [12/20/2021]**
- **URL: [<https://cyberdefenders.org/labs/62>]**
- **Status:[Complete]**

***

## Description

Karen is a security professional looking for a new job. A company called "TAAUSAI"  offered her a position and asked her to complete a couple of tasks to prove her technical competency. Analyze the provided disk image and answer the questions based on your understanding of the cases she was assigned to investigate.

## Tools Used

- FTK Imager
- Autopsy
- RegistryExplorer
- LEcmd
- Regripper 

## Analysis

1. What is the administrator's username?

    **Flag: Karen**

    ![flag1](/CyberDefenders/HireMe/images/Flag1.png)

2. What is the OS's build number?

    **Flag: 16299**

    ![flag2](/CyberDefenders/HireMe/images/Flag2.png)

3. What is the hostname of the computer?

    **Flag: TOTALLYNOTAHACK**

   ![flag3](/CyberDefenders/HireMe/images/Flag3.png)

4. A messaging application was used to communicate with a fellow Alpaca enthusiest. What is the name of the software?

    **Flag: skype**

    ![flag4](/CyberDefenders/HireMe/images/Flag4.png)

5. What is the zip code of the administrator's post?

    **Flag: 19709**

    ![flag5](/CyberDefenders/HireMe/images/Flag5.png)

6. What are the initials of the person who contacted the admin user from TAAUSAI?

    **Flag: MS**

    ![flag6](/CyberDefenders/HireMe/images/Flag6.png)

7. How much money was TAAUSAI willing to pay upfront?

    **Flag: 150000**

    ![flag7](/CyberDefenders/HireMe/images/Flag7.png)

8. What country is the admin user meeting the hacker group in?

    **Flag: Egypt**

    ![flag8](/CyberDefenders/HireMe/images/Flag8.png)

9. What is the machine's timezone? (Use the three-letter abbreviation)

    **Flag: UTC**

    ![flag9](/CyberDefenders/HireMe/images/Flag9.png)

10. When was AlpacaCare.docx last accessed?

    **Flag:  03/17/2019 09:52 PM**

    ![flag10](/CyberDefenders/HireMe/images/Flag10.png)

11. There was a second partition on the drive. What is the letter assigned to it?

    **Flag: A**

    ![flag11](/CyberDefenders/HireMe/images/Flag11.png)

12. What is the answer to the question Company's manager asked Karen?

    **Flag: TheCardCriesNoMore**

    ![flag12](/CyberDefenders/HireMe/images/Flag12.png)

13. What is the job position offered to Karen? (3 words, 2 spaces in between)

    **Flag: cyber security analyst**

    ![flag13](/CyberDefenders/HireMe/images/Flag13.png)

14. When was the admin user password last changed?

    **Flag: 03/21/2019 19:13:09**

    ```text
    This user information is maintained in the F value located in the following path:

    SAM\SAM\Domains\Account\Users\{RID}

    The {RID}, or Relative Identifier, is the portion of a Security Identifier (SID) that identifies a user or group in relation to the authority that issued the SID. Besides providing quite a bit of information about how SIDs are created, Microsoft also provides a list of RIDs (<http://support.microsoft.com/kb/157234>) for well-known users and groups as well as well-known aliases (seen in the SAM\SAM\Domains\Builtin\Aliases key).

    The F value within the key is a binary data type and must be parsed appropriately (see the sam.h file, part of the source code for Peter’s utility) to extract all the information. Some important dates are available in the contents of the binary data for the F value—specifically, several time/date stamps represented as 64-bit FILETIME objects. Those values and their locations are as follows:

    ■ Bytes 8-15 represent the last login date for the account.

    ■ Bytes 24-31 represent the date that the password was last reset (if the password hasn’t been reset or changed, this date will correlate to the account creation date).

    ■ Bytes 32-39 represent the account expiration date.

    ■ Bytes 40-47 represent the date of the last failed login attempt (because the account name has to be correct for the date to be changed on a specific account, this date can also be referred to as the date of the last incorrect password usage).

    Tools such as AccessData’s Registry Viewer will decode this information for you automatically,
    ```

    ![flag14](/CyberDefenders/HireMe/images/Flag14.png)

15. What version of Chrome is installed on the machine?

    **Flag: 72.0.3626.121**

    ![flag15](/CyberDefenders/HireMe/images/Flag15.png)

16. What is the HostUrl of Skype?

    **Flag: <https://download.skype.com/s4l/download/win/Skype-8.41.0.54.exe>**

    ![flag16](/CyberDefenders/HireMe/images/Flag16.png)

17. What is the domain name of the website Karen browsed on Alpaca care that the file AlpacaCare.docx is based on?

    **Flag: palominoalpacafarm.com**

    ![flag17](/CyberDefenders/HireMe/images/Flag17.png)
