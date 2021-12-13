# Challenge Report for Ulysses

- **Challenge Name: [Ulysses]**
- **Date: [12/02/2021]**
- **URL: [https://cyberdefenders.org/labs/41]**
- **Status:[Complete]**

***

## Description

A Linux server was possibly compromised and a forensic analysis is required in order to understand what really happened. Hard disk dumps and memory snapshots of the machine are provided in order to solve the challenge.

Challenge Files:

- victoria-v8.kcore.img: memory dump done by dd’ing /proc/kcore.
- victoria-v8.memdump.img: memory dump done with memdump.
- Debian5_26.zip: volatility custom Linux profile.

## Tools Used

- Volatility
- 010 Editor
- Autopsy

## Analysis

1. The attacker was performing a Brute Force attack. What account triggered the alert?

    **Flag: ulysses**

    Loaded the drive in Autopsy and found answer in /var/log/auth.log

2. How many were failed attempts there?

    **Flag: 32**

    Loaded the drive in Autopsy and found answer in /var/log/auth.log

3. What kind of system runs on the targeted server?

    **Flag: Debian GNU/Linux 5.0**

    Loaded the drive in Autopsy and found answer in /etc/debian_version

4. What is the victim's IP address?

    **Flag: 192.168.56.102**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/Ulysses/victoria-v8.memdump.img --profile=LinuxDebian5_26x86 linux_netstat
    Volatility Foundation Volatility Framework 2.6.1
    UNIX 2190                 udevd/776   
    UDP      0.0.0.0         :  111 0.0.0.0         :    0                           portmap/1429 
    TCP      0.0.0.0         :  111 0.0.0.0         :    0 LISTEN                    portmap/1429 
    UDP      0.0.0.0         :  769 0.0.0.0         :    0                         rpc.statd/1441 
    UDP      0.0.0.0         :38921 0.0.0.0         :    0                         rpc.statd/1441 
    TCP      0.0.0.0         :39296 0.0.0.0         :    0 LISTEN                  rpc.statd/1441 
    UDP      0.0.0.0         :   68 0.0.0.0         :    0                         dhclient3/1624 
    UNIX 5069             dhclient3/1624  
    UNIX 4617              rsyslogd/1661  /dev/log
    UNIX 4636                 acpid/1672  /var/run/acpid.socket
    UNIX 4638                 acpid/1672  
    TCP      ::              :   22 ::              :    0 LISTEN                       sshd/1687 
    TCP      0.0.0.0         :   22 0.0.0.0         :    0 LISTEN                       sshd/1687 
    TCP      ::              :   25 ::              :    0 LISTEN                      exim4/1942 
    TCP      0.0.0.0         :   25 0.0.0.0         :    0 LISTEN                      exim4/1942 
    UNIX 5132                 login/1990  
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :56955 192.168.56.1    : 8888 ESTABLISHED                    nc/2169 
    $ 
    ```

5. What are the attacker's two IP addresses? Flag: comma-separated in ascending order

    **Flag: 192.168.56.1,192.168.56.101**

    Loaded the drive in Autopsy and ran across the IPs in /var/log/exim4/mainlog

6. What is the "nc" service PID number that was running on the server?

    **Flag: 2169**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/Ulysses/victoria-v8.memdump.img --profile=LinuxDebian5_26x86 linux_netstat
    Volatility Foundation Volatility Framework 2.6.1
    UNIX 2190                 udevd/776   
    UDP      0.0.0.0         :  111 0.0.0.0         :    0                           portmap/1429 
    TCP      0.0.0.0         :  111 0.0.0.0         :    0 LISTEN                    portmap/1429 
    UDP      0.0.0.0         :  769 0.0.0.0         :    0                         rpc.statd/1441 
    UDP      0.0.0.0         :38921 0.0.0.0         :    0                         rpc.statd/1441 
    TCP      0.0.0.0         :39296 0.0.0.0         :    0 LISTEN                  rpc.statd/1441 
    UDP      0.0.0.0         :   68 0.0.0.0         :    0                         dhclient3/1624 
    UNIX 5069             dhclient3/1624  
    UNIX 4617              rsyslogd/1661  /dev/log
    UNIX 4636                 acpid/1672  /var/run/acpid.socket
    UNIX 4638                 acpid/1672  
    TCP      ::              :   22 ::              :    0 LISTEN                       sshd/1687 
    TCP      0.0.0.0         :   22 0.0.0.0         :    0 LISTEN                       sshd/1687 
    TCP      ::              :   25 ::              :    0 LISTEN                      exim4/1942 
    TCP      0.0.0.0         :   25 0.0.0.0         :    0 LISTEN                      exim4/1942 
    UNIX 5132                 login/1990  
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :56955 192.168.56.1    : 8888 ESTABLISHED                    nc/2169 
    $ 
    ```

7. What service was exploited to gain access to the system? (one word)

    **Flag: exim4**

8. What is the CVE number of exploited vulnerability?

    **Flag: CVE-2010-4344**

    Loaded the drive in Autopsy and took a look at /var/log/exim4/mainlog.  Then checked [cvedetails](https://www.cvedetails.com/vulnerability-list/vendor_id-10919/product_id-19563/Exim-Exim.html).

9. During this attack, the attacker downloaded two files to the server. Provide the name of the compressed file.

    **Flag: rk.tar**

    Loaded the drive in Autopsy and ran across the IPs in /var/log/exim4/mainlog

10. Two ports were involved in the process of data exfiltration. Provide the port number of the highest one.

    **Flag: 8888**

    ```bash
    $ python2 ~/dev/volatility/vol.py -f ~/VMShared/malware/cyberdefenders.org/Ulysses/victoria-v8.memdump.img --profile=LinuxDebian5_26x86 linux_netstat
    Volatility Foundation Volatility Framework 2.6.1
    UNIX 2190                 udevd/776   
    UDP      0.0.0.0         :  111 0.0.0.0         :    0                           portmap/1429 
    TCP      0.0.0.0         :  111 0.0.0.0         :    0 LISTEN                    portmap/1429 
    UDP      0.0.0.0         :  769 0.0.0.0         :    0                         rpc.statd/1441 
    UDP      0.0.0.0         :38921 0.0.0.0         :    0                         rpc.statd/1441 
    TCP      0.0.0.0         :39296 0.0.0.0         :    0 LISTEN                  rpc.statd/1441 
    UDP      0.0.0.0         :   68 0.0.0.0         :    0                         dhclient3/1624 
    UNIX 5069             dhclient3/1624  
    UNIX 4617              rsyslogd/1661  /dev/log
    UNIX 4636                 acpid/1672  /var/run/acpid.socket
    UNIX 4638                 acpid/1672  
    TCP      ::              :   22 ::              :    0 LISTEN                       sshd/1687 
    TCP      0.0.0.0         :   22 0.0.0.0         :    0 LISTEN                       sshd/1687 
    TCP      ::              :   25 ::              :    0 LISTEN                      exim4/1942 
    TCP      0.0.0.0         :   25 0.0.0.0         :    0 LISTEN                      exim4/1942 
    UNIX 5132                 login/1990  
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :43327 192.168.56.1    : 4444 ESTABLISHED                    sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :   25 192.168.56.101  :37202 CLOSE                          sh/2065 
    TCP      192.168.56.102  :56955 192.168.56.1    : 8888 ESTABLISHED                    nc/2169 
    $ 
    ```

11. Which port did the attacker try to block on the firewall?

    **Flag: 45295**

    One of the downloaded files (rk.tar) included install.sh that had the following command in it.
    ***echo "/usr/sbin/iptables -I OUTPUT 1 -p tcp --dport 45295 -j DROP" >> /etc/init.d/boot.local***

## Lessons Learned

Figuring out the number of login failures in /var/log/auth.log took way to long.  
