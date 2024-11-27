# LetsDefend Walkthrough: Malicious VBA Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-3a1fe918a69d***

![image](https://github.com/user-attachments/assets/c258b802-ce7a-4f14-8145-bd240a68cb4c)

## Challenge Overview
***https://app.letsdefend.io/challenge/Malicious-VBA***

***Scenario:** One of the employees has received a suspicious document attached to the invoice email. They sent you the file to investigate. You managed to extract some strings from the VBA Macro document. Can you refer to CyberChef and decode the suspicious strings?*

*Please, open the document in Notepad++ for security reasons unless you are running the file in an isolated sandbox.*

***Challenge File:** /root/Desktop/ChallengeFile/invoice.vb*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
1. **Preparing the Environment**
   
Begin by locating the `invoice.vb` file, which is a Visual Basic script. in Windows, open it in a safe text editor, like Notepad++, to examine its contents. This ensures you don’t accidentally execute any malicious code.

In Unix-based systems, use the terminal and run the command:

```bash
strings invoice.vb
```

The `strings` command is a common Unix/Linux utility used to extract printable strings from a binary file. When you execute `strings invoice.vb`, it scans the `invoice.vb` file, and identifies sequences of characters that are likely to be human-readable text. Copy the contents in the text editor.

![image](https://github.com/user-attachments/assets/0516990a-bb83-419a-b58d-7e623135dbd9)

Upon inspection, the file contains unreadable and encoded strings embedded within VBA macro functions. For example, the file includes a function, `hgmneqolwxg`, which seems to decode a series of encoded strings. These strings need to be decoded to understand the payload’s behavior.

2. **Decoding with CyberChef*8
   
Unfortunately, there’s no definitive way to automatically determine which character encoding it is. I used [CyberChef](https://gchq.github.io/CyberChef/) and tried different decoders before, eventually seeing the text by the CharCode decoder.

Now, you just need to identify the necessary encoded string and decode it using the CyberChef CharCode decoder.

***Question - What website is hosting the malicious payload?***

Look for human unreadable strings in the file. Try with the encoded strings in line 10:

![image](https://github.com/user-attachments/assets/e01fd99d-de5b-498b-b6ef-4a482d5e9a1e)

![image](https://github.com/user-attachments/assets/add8e34a-0347-4d50-89c7-76d268b26997)

```bash
https://tinyurl.com/g2z2gh6f
```

***Question - What is the filename of the payload?***

After that, try the encoded strings present in next line 11:

![image](https://github.com/user-attachments/assets/dcb99318-73c8-42fd-8e55-ffdaabf5761c)

![image](https://github.com/user-attachments/assets/917e97f0-4682-41c5-aa42-71152ef77a5f)

```bash
dropped.exe
```

***Question - What method is it using to establish an HTTP connection between files on the malicious web server?***

Going back to the VBA file, try to decode the next encoded strings present in line 13:

![image](https://github.com/user-attachments/assets/c5c80d89-3848-4dc2-8bed-8ffafa471e56)

![image](https://github.com/user-attachments/assets/1feffc6c-4733-4153-a911-b37ac7e96815)

```bash
MSXML2.ServerXMLHTTP
```

***Question - What user-agent string is it using?***

After this, try to decode the next encoded strings present in line 15:

![image](https://github.com/user-attachments/assets/c5c2d7bf-fd22-407a-bc3d-f47365162b6f)

![image](https://github.com/user-attachments/assets/2de0dbf1-a3e2-4c1c-be0c-086370605bbb)

```bash
Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)
```

***Question - What object does the attacker use to be able to read or write text and binary files?***

After this, try to decode the next encoded strings present in line 19:

![image](https://github.com/user-attachments/assets/4721b202-b3ce-4590-bfc4-593e1dbb319b)

![image](https://github.com/user-attachments/assets/7ae43650-7ea5-4f1f-adb2-c01824f3c226)

```bash
ADODB.Stream
```

***Question - What is the object the attacker uses for WMI execution? Possibly they are using this to hide the suspicious application running in the background.***

Finally, try to decode the next encoded strings present in lines 69–73:

![image](https://github.com/user-attachments/assets/71765947-a52f-4b10-be3e-e0151cd2b7e4)

![image](https://github.com/user-attachments/assets/ddd7c5b2-ea06-4e48-87e8-590ec6d6c002)

It looks correct, but there are some repeated strings in the output. Try to see the output for the encoded strings present just in line 73:

![image](https://github.com/user-attachments/assets/db288cc8-299c-4dcc-b7b1-f3a27da901cd)

![image](https://github.com/user-attachments/assets/0d4f6dd7-1b4a-4b29-b202-a63a0550fad4)

```bash
winmgmts:\\.\root\cimv2:Win32_Process
```

![image](https://github.com/user-attachments/assets/6f6e5f91-f6de-4007-b9bf-ada7e7ea91d6)

