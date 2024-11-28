# LetsDefend Walkthrough: MSHTML Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-0b75fa2cdfc2***

![image](https://github.com/user-attachments/assets/3ee6838b-6936-4459-bf6d-a50582118386)

## Challenge Overview
***https://app.letsdefend.io/challenge/mshtml***

***Scenario:** Extract critical IOCs from each and determine the vulnerability they exploit.*

***Challenge Files:***

*/root/Desktop/ChallengeFile/Employee_W2_Form.docx*  
*/root/Desktop/ChallengeFile/Employees_Contact_Audit_Oct_2021.docx*  
*/root/Desktop/ChallengeFile/Work_From_Home_Survey.doc*  
*/root/Desktop/ChallengeFile/income_tax_and_benefit_return_2021.docx*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
In this challenge, we analyze the four malicious document files to uncover hidden IP addresses, malicious domains, and ultimately the vulnerability they exploit.

![image](https://github.com/user-attachments/assets/d2a77125-37a4-4b88-a80f-bb37e00c3b3b)

Using the **Didier Stevens’** tools provided to us, we’ll break down each sample step-by-step, hunting for Indicators of Compromise (IOCs) and unraveling the mechanics of this exploit.

![image](https://github.com/user-attachments/assets/95bb4474-5fcd-4408-b463-037fa1c55e76)

***Question - Examine the Employees_Contact_Audit_Oct_2021.docx file, what is the malicious IP in the docx file?***

To start, we focus on the `Employees_Contact_Audit_Oct_2021.docx` file. Since a `.docx` file is essentially a compressed ZIP archive, we use the `zipdump.py` tool to extract its contents using the following command:

```bash
python3 zipdump.py /root/Desktop/ChallengeFiles/Employees_Contact_Audit_Oct_2021.docx
```

![image](https://github.com/user-attachments/assets/27b12ce4-f9a0-4c0f-8eab-404054b558a8)

This reveals the document’s internal structure. With numerous embedded files, we take a broader approach, dumping all data using the `-D` flag:

```bash
python3 zipdump.py -D /root/Desktop/ChallengeFiles/Employees_Contact_Audit_Oct_2021.docx
```

We pipe this output into the `re-search.py` tool, a tool designed for searching patterns such as IPs or domains. Using its built-in `ipv4` filter, we identify the malicious IP address:

```bash
python3 zipdump.py -D /root/Desktop/ChallengeFiles/Employees_Contact_Audit_Oct_2021.docx | python3 re-search.py -n -u ipv4
```

![image](https://github.com/user-attachments/assets/b0fc7f63-e3ef-439c-b0e5-9922b86407d9)

```bash
175.24.190.249
```

***Question - Examine the Employee_W2_Form.docx file, what is the malicious domain in the docx file?***

Next, we analyze `Employee_W2_Form.docx` using the same workflow. After extracting its contents with `zipdump.py`, we switch the `re-search.py` filter to locate domain patterns. However, this time, we don’t get results with common filters like `url`.

![image](https://github.com/user-attachments/assets/bc327a21-2ba7-443c-96a3-f72aae29fc1b)

Trying with the `domaintld` filter:

```bash
python3 zipdump.py -D /root/Desktop/ChallengeFiles/Employee_W2_Form.docx | python3 re-search.py -u -n domaintld
```

![image](https://github.com/user-attachments/assets/7a189e8f-006a-4c70-9457-3debe9125fd8)

We successfully uncover the malicious domain.

```bash
arsenal.30cm.tw
```

***Question - Examine the Work_From_Home_Survey.doc file, what is the malicious domain in the doc file?***

Analyzing a `.doc` file differs slightly because this format predates `.docx` and isn’t a ZIP archive. We use `zipdump.py` to examine the streams individually.

![image](https://github.com/user-attachments/assets/099336e3-2203-445d-b99c-ccd390714058)

According to the Office Open XML (OOXML) WordProcessingML File:

![image](https://github.com/user-attachments/assets/80aa55a9-c80a-49ac-9629-6e2296217a09)

stream 10 often contains references to external URLs. We run:

```bash
python3 zipdump.py /root/Desktop/ChallengeFiles/Work_From_Home_Survey.doc -s 10 -d
```

![image](https://github.com/user-attachments/assets/4590ce83-4d57-455f-9e51-6d6bef654750)

The output appears encoded, so we pipe it into another Didier Stevens tool `numbers-to-string.py`, to decode the hidden strings:

```bash
python3 zipdump.py /root/Desktop/ChallengeFiles/Work_From_Home_Survey.doc -s 10 -d | python3 numbers-to-string.py
```

![image](https://github.com/user-attachments/assets/7a3b801b-eee9-4f19-9b5b-d5d0d10d3a47)

```bash
trendparlye.com
```

***Question - Examine the income_tax_and_benefit_return_2021.docx, what is the malicious domain in the docx file?***

Returning to `.docx` file analysis, we follow a similar process as before, this time searching for domains using either the `url-domain` or the `domaintld` filter of `re-search.py`:

```bash
python3 zipdump.py -D /root/Desktop/ChallengeFiles/income_tax_and_benefit_return_2021.docx | python3 re-search.py -n -u domaintld

                                 ---OR---

python3 zipdump.py -D /root/Desktop/ChallengeFiles/income_tax_and_benefit_return_2021.docx | python3 re-search.py -n -u url-domain
```

![image](https://github.com/user-attachments/assets/9c823538-9ee3-400f-814d-901a32159a04)

```bash
hidusi.com
```

***Question - What is the vulnerability the above files exploited?***

Now that we’ve identified the IOCs, it’s time to link them to the exploited vulnerability. We submit the SHA-256 hashes of each of the provided files to [VirusTotal](https://www.virustotal.com/gui/home/upload):

![image](https://github.com/user-attachments/assets/1cbb6507-889a-4946-a5de-b90418f55729)

![image](https://github.com/user-attachments/assets/d9693235-50dd-43cf-a5ee-43442cce0cb8)

![image](https://github.com/user-attachments/assets/3cf65ba5-6f3c-42f8-a79c-02d522c200d4)

![image](https://github.com/user-attachments/assets/fa9651b7-f1e3-4076-a089-5825746dafc2)

![image](https://github.com/user-attachments/assets/033a32a5-1911-4819-9a13-21d067814fdf)

Submitting the hashes reveals that all the files are consistently tagged with one common vulnerability. This zero-day is a **remote code execution vulnerability** in MSHTML, a rendering engine used by Microsoft Office documents to host ActiveX controls. Attackers craft malicious ActiveX components within Office documents and trick users into opening them, leading to arbitrary code execution.

```bash
CVE-2021–40444
```

![image](https://github.com/user-attachments/assets/c94cb632-29c5-4773-bb73-dddffb73fe7f)

