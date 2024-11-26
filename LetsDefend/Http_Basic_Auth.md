# LetsDefend Walkthrough: Http Basic Auth. Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-90e86857ca0a***

![image](https://github.com/user-attachments/assets/0c66879d-4235-48e4-a6fd-8b6f579f77c9)

## Challenge Overview
***https://app.letsdefend.io/challenge/http-basic-auth***

***Scenario:** We receive a log indicating a possible attack, can you gather information from the .pcap file?*

***Log file:** /root/Desktop/ChallengeFile/webserver.em0.pcap*

***Note:** pcap file found public resources.*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
***Question - How many HTTP GET requests are in pcap?***

Filter the captured packets for HTTP GET requests:

```bash
http.request.method=="GET"
```

![image](https://github.com/user-attachments/assets/9fbb398f-30f3-4047-894f-4fce0b162f89)

```bash
5
```

***Question - What is the server operating system?***

Follow the HTTP Stream of the filtered GET packets.

![image](https://github.com/user-attachments/assets/6df0d23e-dcb1-48c7-a117-e3cde88d6b4e)

![image](https://github.com/user-attachments/assets/41a97539-79ff-4c4e-a0e1-f1701226d9de)

```bash
FreeBSD
```

***Question - What is the name and version of the web server software?***

```bash
Apache/2.2.15
```

***Question - What is the version of OpenSSL running on the server?***

```bash
OpenSSL/0.9.8n
```

***Question - What is the client's user-agent information?***

![image](https://github.com/user-attachments/assets/42518ded-6b5f-4ebb-9094-fc4c7f0e2804)

```bash
Lynx/2.8.7rel.1 libwww-fm/2.14 ssl-mm/1.4.1 openssl/0.9.8n
```

***Question - What is the username used for Basic Authentication?***

By examining the stream, we see that the response is `200 OK`. Now, if we examine the packet headers, we can either go with `Authorization: Basic d2ViYWRtaW46VzNiNERtMW4=`

The base64 decoder in CyberChef gives the answer in the form of `Username:Password`.

![image](https://github.com/user-attachments/assets/100e54e4-421b-4520-91c8-84374fefe735)

Alternatively, you can check the `Credentials` section of the packet's HTTP header, which displays the Username and Password in plain text.


![image](https://github.com/user-attachments/assets/130e9701-db2b-449e-9dd6-797d90934322)

```bash
webadmin
```

***Question - What is the user password used for Basic Authentication?***

```bash
W3b4Dm1n
```

![image](https://github.com/user-attachments/assets/39d031f3-f6ef-4e11-9f8c-b870d7ce2f17)
