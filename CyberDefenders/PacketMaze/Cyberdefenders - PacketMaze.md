# Challenge report for PacketMaze

- **Challenge Name: PacketMaze**
- **Date: Done before I started documenting.**
- **URL: <https://cyberdefenders.org/labs/68>**
- **Status: Complete**

***

## Description

As an analyst working for a security service provider, you have been tasked with analyzing a packet capture for a customer's employee whose network activity has been monitored for a while -possible insider.

## Tools Used

- WireShark

## Analysis

1. What is the FTP password?

    **AfricaCTF2021**

    ```text
    Load in WireShark, filter on ftp, and search for PASS.
    ```

2. What is the IPv6 address of the DNS server used by 192.168.1.26? (####::####:####:####:####)

    **fe80::c80b:adff:feaa:1db7**

    ```text
    Filter by dns, look for IPv6 addresses.
    ```

3. What domain is the user looking up in packet 15174?

    **www.7-zip.org**

    ```text
    Go to packet 15174 and look at DNS Query.
    ```

4. How many UDP packets were sent from 192.168.1.26 to 24.39.217.246?

    **10**

    ```text
    Wireshark -> Statistics -> Conversations -> UDP
    ```

5. What is the MAC address of the system being monitored?

    **c8:09:a8:57:47:93**

    ```text
    Look at any packet to/from 92.168.1.26 under ethernet.
    ```

6. What was the camera model name used to take picture 20210429_152157.jpg ?

    **LM-Q725K**

    ```text
    The file was transfered via FTP so filter via ftp-data, follow the stream, and  then check the metadata.
    ```

7. What is the server certificate public key that was used in TLS session:
da4a0000342e4b73459d7360b4bea971cc303ac18d29b99067e46d16cc07f4ff?

    **04edcc123af7b13e90ce101a31c2f996f471a7c8f48a1b81d765085f548059a550f3f4f62ca1f0e8f74d727053074a37bceb2cbdc7ce2a8994dcd76dd6834eefc5438c3b6da929321f3a1366bd14c877cc83e5d0731b7f80a6b80916efd4a23a4d**

    ```text
    Filter;  tls.handshake.session_id == da:4a:00:00:34:2e:4b:73:45:9d:73:60:b4:be:a9:71:cc:30:3a:c1:8d:29:b9:90:67:e4:6d:16:cc:07:f4:ff
    ```

8. What is the first TLS 1.3 client random that was used to establish a connection with protonmail.com?

    **24e92513b97a0348f733d16996929a79be21b0b1400cd7e2862a732ce7775b70**

    ```text
    Filter for tls to protonmail.com; tls && ip.dst==185.70.41.35 and check first packet.
    ```

9. What country is the MAC address of the FTP server registered in? (two words, one space in between)

    **United States**

10. What time was a non-standard folder created on the FTP server on the 20th of April? (hh:mm)

    **17:53**

    ```text
    filter on ftp-data and follow the stream.
    ```

11. What domain was the user connected to in packet 27300?

    **dfir.science**

    ```text
    Have Name Resolution turned on under View -> Name Resolution and go to the packet.
    ```
