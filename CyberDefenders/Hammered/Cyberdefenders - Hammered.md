# Challenge Report for xxxxxx

- **Challenge Name: [Hammered]**
- **Date: [11/03/2022]**
- **URL: [https://cyberdefenders.org/blueteam-ctf-challenges/42]**
- **Status:[Active]**

***

## Description

This challenge takes you into the world of virtual systems and confusing log data. In this challenge, figure out what happened to this webserver honeypot using the logs from a possibly compromised server.

Thanks, th3c0rt3x for reviewing the challenge.

## Tools Used

## Analysis

1. Which service did the attackers use to gain access to the system?

    **Flag:ssh**

    ```text
    Checked auth.log for failures.  Huge amount for ssh.
    ```

2. What is the operating system version of the targeted system? (one word)

    **Flag:4.2.4-1ubuntu3**

    ```text
    Foudn in dmesg; [    0.000000] Linux version 2.6.24-26-server (buildd@crested) (gcc version 4.2.4 (Ubuntu 4.2.4-1ubuntu3)) #1 SMP Tue Dec 1 18:26:43 UTC 2009 (Ubuntu 2.6.24-26.64-server)
    ```

3. What is the name of the compromised account?

    **Flag:root**

4. Consider that each unique IP represents a different attacker. How many attackers were able to get access to the system?

    **Flag:6**

    WHY???  When I compart who was trying vs who got in I get 7?

    ```bash
    $ grep "Invalid user" auth.log |  awk '{print $10}' | sort | uniq
    114.80.166.219
    116.6.19.70
    121.11.66.70
    122.165.9.200
    122.226.202.12
    124.207.117.9
    124.51.108.68
    125.235.4.130
    173.9.147.165
    190.166.87.164
    201.64.234.2
    203.81.226.86
    210.68.70.170
    211.154.254.248
    217.15.55.133
    218.56.61.114
    219.150.161.20
    220.170.79.247
    222.169.224.197
    222.66.204.246
    24.192.113.91
    24.94.90.96
    58.17.30.49
    59.46.39.148
    61.168.227.12
    65.208.122.48
    8.12.45.242
    83.216.63.124
    $ grep "Accepted password for root" auth.log |  awk '{print $11}' | sort | uniq
    10.0.1.2
    121.11.66.70
    122.226.202.12
    151.81.204.141
    151.81.205.100
    151.82.3.201
    188.131.22.69
    188.131.23.37
    190.166.87.164
    190.167.70.87
    190.167.74.184
    193.1.186.197
    201.229.176.217
    219.150.161.20
    222.169.224.197
    222.66.204.246
    61.168.227.12
    94.52.185.9
    ```

5. Which attacker's IP address successfully logged into the system the most number of times?

    **Flag:219.150.161.20**

    ```bash
    $ grep "Accepted password for root" auth.log 
    Mar 29 13:27:26 app-1 sshd[21556]: Accepted password for root from 10.0.1.2 port 51784 ssh2
    Apr 19 05:41:44 app-1 sshd[8810]: Accepted password for root from 219.150.161.20 port 51249 ssh2
    Apr 19 05:42:27 app-1 sshd[9031]: Accepted password for root from 219.150.161.20 port 40877 ssh2
    Apr 19 05:55:20 app-1 sshd[12996]: Accepted password for root from 219.150.161.20 port 55545 ssh2
    Apr 19 05:56:05 app-1 sshd[13218]: Accepted password for root from 219.150.161.20 port 36585 ssh2
    Apr 19 10:45:36 app-1 sshd[28030]: Accepted password for root from 222.66.204.246 port 48208 ssh2
    Apr 19 11:03:44 app-1 sshd[30277]: Accepted password for root from 201.229.176.217 port 54465 ssh2
    Apr 19 11:15:26 app-1 sshd[30364]: Accepted password for root from 190.167.70.87 port 49497 ssh2
    Apr 19 22:37:24 app-1 sshd[2012]: Accepted password for root from 190.166.87.164 port 50753 ssh2
    Apr 19 22:54:06 app-1 sshd[2149]: Accepted password for root from 190.166.87.164 port 51101 ssh2
    Apr 19 23:02:25 app-1 sshd[2210]: Accepted password for root from 190.166.87.164 port 51303 ssh2
    Apr 20 06:13:03 app-1 sshd[26712]: Accepted password for root from 121.11.66.70 port 33828 ssh2
    Apr 21 11:51:38 app-1 sshd[2649]: Accepted password for root from 193.1.186.197 port 38318 ssh2
    Apr 21 11:56:37 app-1 sshd[2686]: Accepted password for root from 151.81.205.100 port 54272 ssh2
    Apr 22 01:30:27 app-1 sshd[4877]: Accepted password for root from 151.82.3.201 port 49249 ssh2
    Apr 22 06:41:38 app-1 sshd[5876]: Accepted password for root from 151.81.204.141 port 59064 ssh2
    Apr 22 11:02:15 app-1 sshd[7940]: Accepted password for root from 222.169.224.197 port 45356 ssh2
    Apr 23 03:11:03 app-1 sshd[13633]: Accepted password for root from 122.226.202.12 port 40892 ssh2
    Apr 23 03:20:41 app-1 sshd[13930]: Accepted password for root from 122.226.202.12 port 40209 ssh2
    Apr 24 11:36:19 app-1 sshd[24436]: Accepted password for root from 121.11.66.70 port 58832 ssh2
    Apr 24 15:28:37 app-1 sshd[31338]: Accepted password for root from 61.168.227.12 port 43770 ssh2
    Apr 24 16:33:36 app-1 sshd[31845]: Accepted password for root from 188.131.22.69 port 1844 ssh2
    Apr 24 19:15:54 app-1 sshd[32299]: Accepted password for root from 190.167.74.184 port 60992 ssh2
    Apr 25 10:38:56 app-1 sshd[9560]: Accepted password for root from 94.52.185.9 port 59821 ssh2
    Apr 26 04:42:55 app-1 sshd[20096]: Accepted password for root from 188.131.23.37 port 3527 ssh2
    Apr 26 04:59:02 app-1 sshd[20491]: Accepted password for root from 188.131.23.37 port 3561 ssh2
    Apr 26 08:47:28 app-1 sshd[23501]: Accepted password for root from 188.131.23.37 port 4271 ssh2
    Apr 26 08:51:50 app-1 sshd[23542]: Accepted password for root from 188.131.23.37 port 4280 ssh2
    ```

6. How many requests were sent to the Apache Server?

    **Flag:365**

    ```bash
    $ cat www-access.log | wc -l
    365
    ```

7. How many rules have been added to the firewall?

    **Flag:6**

    ```bash
    $ grep iptables *
    grep: apache2: Is a directory
    grep: apt: Is a directory
    auth.log:Apr 15 12:49:09 app-1 sudo:   user1 : TTY=pts/0 ;? PWD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee ../templates/proxy/iptables.conf
    auth.log:Apr 15 15:06:13 app-1 sudo:   user1 : TTY=pts/1 ; PWD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee ../templates/proxy/iptables.conf
    auth.log:Apr 15 15:17:45 app-1 sudo:   user1 : TTY=pts/1 ; PWD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee ../templates/proxy/iptables.conf
    auth.log:Apr 15 15:18:23 app-1 sudo:   user1 : TTY=pts/1 ; PWD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee ../templates/proxy/iptables.conf
    auth.log:Apr 24 19:25:37 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -L
    auth.log:Apr 24 20:03:06 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p ssh -dport 2424 -j ACCEPT
    auth.log:Apr 24 20:03:44 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p tcp -dport 53 -j ACCEPT
    auth.log:Apr 24 20:04:13 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p udp -dport 53 -j ACCEPT
    auth.log:Apr 24 20:06:22 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p tcp --dport ssh -j ACCEPT
    auth.log:Apr 24 20:11:00 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p tcp --dport 53 -j ACCEPT
    auth.log:Apr 24 20:11:08 app-1 sudo:     root : TTY=pts/2 ; PWD=/etc ; USER=root ; COMMAND=/sbin/iptables -A INPUT -p tcp --dport 113 -j ACCEPT
    ```

8. One of the downloaded files to the target system is a scanning tool. Provide the tool name.

    **Flag:nmap**

    ```text
    Manually dig through apt/term.log until I run into...

    Log started: 2010-04-24  19:38:15
    Selecting previously deselected package nmap.
    (Reading database ... 26973 files and directories currently installed.)
    Unpacking nmap (from .../archives/nmap_4.53-3_amd64.deb) ...
    Setting up nmap (4.53-3) ...
    ```

9. When was the last login from the attacker with IP 219.150.161.20? Format: MM/DD/YYYY HH:MM:SS AM

    **Flag:04/19/2010 05:56:05 AM**

    First we find the date/time and then we figure out the year.

    ```bash
    $grep "219.150.161.20" auth.log | grep "Accepted password for"
    Apr 19 05:41:44 app-1 sshd[8810]: Accepted password for root from 219.150.161.20 port 51249 ssh2
    Apr 19 05:42:27 app-1 sshd[9031]: Accepted password for root from 219.150.161.20 port 40877 ssh2
    Apr 19 05:55:20 app-1 sshd[12996]: Accepted password for root from 219.150.161.20 port 55545 ssh2
    Apr 19 05:56:05 app-1 sshd[13218]: Accepted password for root from 219.150.161.20 port 36585 ssh2
    $ls -al auth.log
    -rw-r----- 1 eddy eddy 10327345 Jul  3  2010 auth.log
    ```

10. The database displayed two warning messages, provide the most important and dangerous one.

    **Flag:mysql.user contains 2 root accounts without password!**

    So lets search for WARNING and see what we come up with.

    ```bash
    $ grep WARNING daemon.log
    Mar 18 10:18:42 app-1 /etc/mysql/debian-start[7566]: WARNING: mysql.user contains 2 root accounts without password!
    ```

11. Multiple accounts were created on the target system. Which one was created on Apr 26 04:43:15?

    **Flag:wind3str0y**

    ```bash
    $ grep "Apr 26 04:43:15" auth.log
    Apr 26 04:43:15 app-1 groupadd[20114]: new group: name=wind3str0y, GID=1005
    Apr 26 04:43:15 app-1 useradd[20115]: new user: name=wind3str0y, UID=1004, GID=1005, home=/home/wind3str0y, shell=/bin/bash
    ```

12. Few attackers were using a proxy to run their scans. What is the corresponding user-agent used by this proxy?

    **Flag:pxyscand/2.1**

    ```bash
    $ cat www-access.log | awk '{print $12}' | sort | uniq
    "-"
    "Apple-PubSub/65.12.1"
    "Mozilla/4.0
    "Mozilla/5.0
    "pxyscand/2.1"
    "WordPress/2.9.2;
    ```

## Lessons Learned
