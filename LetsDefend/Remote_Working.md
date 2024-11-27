# LetsDefend Walkthrough: Remote Working Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-fb9352840e9f***

![image](https://github.com/user-attachments/assets/eda7993d-451b-418b-8d2c-eaa01e5c50d8)

## Challenge Overview
***https://app.letsdefend.io/challenge/remote-working***

***Scenario:** Analysis XLS File*

***Challenge File:** /root/Desktop/ChallengeFile/ORDER_SHEET_SPEC.zip*

***Password:** infected*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
The first step is to locate the `ORDER_SHEET_SPEC.zip` file and use the password `infected` to extract its contents, revealing a `ORDER_SHEET_SPEC.xlsm` file. Upload the `ORDER_SHEET_SPEC.xlsm` file to an OSINT malware analysis platform like [VirusTotal](https://www.virustotal.com/gui/home/upload).

![image](https://github.com/user-attachments/assets/c1ac6851-1b85-4a2f-9677-a0a0b278f784)

***Question - What is the date the file was created?***

In the **DETAILS** tab of VirusTotal, scroll down to the History section. There, you will find the “Creation Time” of the file.

![image](https://github.com/user-attachments/assets/880fcc6e-25d7-4145-973a-b1426f419301)

```bash
2020-02-01 18:28:07
```

***Question - With what name is the file detected by Bitdefender antivirus?***

Reopen the **DETECTION** tab on VirusTotal. This tab lists the antivirus engines that scanned the file, along with their detections. Locate the entry for **Bitdefender** in the left-hand column.

![image](https://github.com/user-attachments/assets/7e169df1-6d91-472a-af0a-d45a8580a388)

```bash
Trojan.GenericKD.36266294
```

***Question - How many files are dropped on the disk?***

To determine how many files are dropped on the disk, refer to the “Files Dropped” section in the **BEHAVIOUR** tab. Since VirusTotal only considers unique dropped files based on their origin and context:

1. C:\ProgramData\Podaliri4.exe
2. C:\Users\aETAdzjz\AppData\Local\Temp\xx.vbs
3. C:\users\administrator\appdata\local\microsoft\windows\temporary internet files\content.mso485d1763.emf

![image](https://github.com/user-attachments/assets/a860807c-b265-4186-832c-438e91a3d431)

```bash
3
```

***Question - What is the sha-256 hash of the file with emf extension it drops?***

Within the same “Files Dropped” section, identify the file with the .emf extension and get the corresponding **SHA-256 hash** provided along with it.

![image](https://github.com/user-attachments/assets/026a7234-80a1-486b-8672-c6772ba1693a)

```bash
979dde2aed02f077c16ae53546c6df9eed40e8386d6db6fc36aee9f966d2cb82
```

***Question - What is the exact url to which the relevant file goes to download spyware?***

Under the **RELATIONS** tab in VirusTotal, you will find a list of URLs and domains the malicious file attempts to contact during execution. The **Contacted URLs** section, lists the exact URLs used for downloading spyware or other payloads. The second URL is flagged as malicious by numerous vendors and hence, the status code 403 or “Forbidden”.

![image](https://github.com/user-attachments/assets/240d2800-f88e-4405-bc6d-61a565eab1ac)

```bash
https://multiwaretecnologia.com.br/js/Podaliri4.exe
```

![image](https://github.com/user-attachments/assets/6dd2fe21-9d90-4338-b5d4-a216cab2e814)
