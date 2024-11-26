# LetsDefend Walkthrough: Upstyle Backdoor Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-8cd8afd615d2***

![Upstyle-badge](https://github.com/user-attachments/assets/ccf3b309-0aa1-47a4-8097-a722dba240c4)

## Challenge Overview
***https://app.letsdefend.io/challenge/kernel-exploit***

***Scenario:** Help us to analyze specifically targeting a backdoor known as UPSTYLE and its relation to CVEs (Common Vulnerabilities and Exposures) that affect Palo Alto Networks’ products.*

***File Location:** C:\Users\LetsDefend\Desktop\ChallengeFile\sample.zip*

***File Password:** infected*

***Note:** To avoid potential malware infections on your machine, please use a hypervisor (virtual machine) when downloading and analyzing files.*

## Solution
1. **Extracting the File**
The first step is to extract the provided ZIP file using the password `infected`. Once extracted, you should have a Python script ready for analysis.

2. **Understanding the Script Structure**
Open the Python script in your favorite code editor. The script is structured to perform several malicious actions, including monitoring a log file for embedded commands, executing these commands, and taking steps to avoid detection.

3. **Key Functions and Paths**

![image](https://github.com/user-attachments/assets/62d53dc6-53a7-4781-9c47-3c9b872e358a)

***Question - What function is responsible for monitoring a log file for embedded commands and executing them, while also restoring the file to its original state? Answer Format: functionname()***

```bash
check()
```

The `check()` function reads the log file, looks for a specific pattern (base64 encoded within `img` tags), decodes the command, executes it, and writes the output to the specified CSS file.

The script includes a restoration function to maintain the original state of the log file, thus evading detection.

![image](https://github.com/user-attachments/assets/71464802-21db-4f02-908c-265e48c7d57b)

***Question - What is the system path that is used by the threat actor?***

![image](https://github.com/user-attachments/assets/36d5276b-4a87-4551-bd85-8fd23c9b2518)

```bash
/usr/lib/python3.6/site-packages/system.pth
```

***Question - What is the CSS path used by the script?***

```bash
css_path = '/var/appweb/sslvpndocs/global-protect/portal/css/bootstrap.min.css'
content = open(css_path).read()
atime = os.path.getatime(css_path)
mtime = os.path.getmtime(css_path)
```

This is where the threat actor will see the output of their commands:

```bash
/var/appweb/sslvpndocs/global-protect/portal/css/bootstrap.min.css
```

***Question - Where does the script attempt to remove certain license files from?***

![image](https://github.com/user-attachments/assets/b10a8cc0-9e1e-47b2-a1e3-a7ba14ebddf6)

The script interacts with the system path and attempts to delete certain license files.

```bash
/opt/pancfg/mgmt/licenses/
```

***Question - What specific signal does the protection function respond to?***

![image](https://github.com/user-attachments/assets/b7621ed0-fa9f-4dc8-a9c9-9a6cc830adf1)

The script uses a protection function `protect()` to listen for the SIGTERM signal and restore the system path file if it's deleted.

```bash
SIGTERM
```

***Question - What function is responsible for protecting the script itself? Answer Format: functionname()***

```bash
protect()
```

***Question - What type of pattern does the script search for within the log file?***

```bash
img\[([a-zA-Z0-9+/=]+)\]
```

***Question - Which specific log file does the script read from?***

```bash
/var/log/pan/sslvpn_ngx_error.log
```

![image](https://github.com/user-attachments/assets/885a6f32-b897-45ab-b8a5-b6970a8d2d0c)
