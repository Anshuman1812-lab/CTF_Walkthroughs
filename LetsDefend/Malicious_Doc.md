# LetsDefend Walkthrough: Malicious Doc Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-3d51f86ec12b***

![image](https://github.com/user-attachments/assets/4ed47e12-bcc8-4972-b65e-6c756805431d)

## Challenge Overview
***https://app.letsdefend.io/challenge/malicious-doic***

***Scenario:** Analyze malicious .doc file*

***Challenge File:** /root/Desktop/ChallengeFile/factura.zip*

***Password:** infected*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
1. **Setting Up the Environment**
   
The first step is to download the `factura.zip` file from the challenge portal. Use the password `infected` to extract its contents, revealing a `factura.doc` file.

![image](https://github.com/user-attachments/assets/f7c0be43-5f91-4b6f-bc40-c5c8fbc0d718)

2. **Identifying the Exploit Type**
   
Upload the `factura.doc` file to an OSINT malware analysis platform like [VirusTotal](https://www.virustotal.com/gui/home/upload). After the scan is completed, the results show various vendor analyses. As the hint suggested that the exploit is a Rich Text Format (RTF) file, I started looking for an exploit that began with `rtf`.

![image](https://github.com/user-attachments/assets/c2c8c7d4-f615-43e7-a199-b93ac0583c5c)

***Question - What type of exploit is running as a result of the relevant file running on the victim machine?***

```bash
Rtf.Exploit
```

3. **Determining the Exploit CVE**
   
Further analysis in VirusTotal reveals the relevant CVE code multiple times in the scan results.

![image](https://github.com/user-attachments/assets/a60411c8-ea3c-47bf-a09f-daa8f9764ff9)

***Question - What is the relevant Exploit CVE code obtained as a result of the analysis?***

```bash
CVE-2017–11882
```

CVE-2017-11882 is a well-known vulnerability in Microsoft Office's Equation Editor. This vulnerability allows attackers to execute arbitrary code on a victim's machine when the malicious file is opened.

4. **Identifying Downloaded Malware**
   
Navigate to the **Relations** tab in VirusTotal to explore external connections established by the malicious `factura.doc` file. The analysis highlights a URL contacted by the file, which downloads a malicious executable.

![image](https://github.com/user-attachments/assets/b2294729-1e21-4326-9dd5-1304273ea821)

***Question - What is the name of the malicious software downloaded from the internet as a result of the file running?***

```bash
jan2.exe
```

5. **Network Communications**
   
Switch to the **Behavior** tab in VirusTotal to examine the network activity associated with the `factura.doc` file. Scroll through the IP traffic data to find the IP address and port that the malware communicates with.

![image](https://github.com/user-attachments/assets/263bb3de-4589-4a5c-aa85-d5b2da8f0d58)

***Question - What is the IP address and port information it communicates with?***

```bash
185.36.74.48:80
```

6. **Dropped File on Disk**
   
Finally, return to the **Relations** tab to identify any files the malicious executable dropped onto the victim’s disk. This tab reveals a list of dropped files along with their SHA-256 hashes.

![image](https://github.com/user-attachments/assets/219d3bac-24b7-4f79-b28f-b224ffdd4662)

Scrolling down and inspecting the `Win32 EXE` file type, the Sha-256 hash leads to other associated executable files.

![image](https://github.com/user-attachments/assets/b2fdd454-159e-4774-88bb-00f8a797ab4b)

***Question - What is the exe name it drops to disk after it runs?***

```bash
aro.exe
```

![image](https://github.com/user-attachments/assets/23436b35-6910-469c-8585-bccb650ca421)
