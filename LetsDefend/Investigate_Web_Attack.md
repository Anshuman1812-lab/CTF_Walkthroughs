# LetsDefend Walkthrough: Investigate Web Attack Challenge
***https://medium.com/@anshumansingh_71873/letsdefend-walkthrough-1ba89d5b0734***

![image](https://github.com/user-attachments/assets/aff5fdbf-ed40-4167-a876-0ac530b89def)

## Challenge Overview
***https://app.letsdefend.io/challenge/investigate-web-attack***

***Scenario:** We detected some web attacks and need to do a deep investigation.*

***Challenge File:** /root/Desktop/ChallengeFile/access.log*

## Solution
A preliminary analysis of the log file identified quite a few clues about potential malicious activity:

- The User-Agent string reveals that `Nikto/2.1.6` is being used which is a popular web vulnerability scanner, which confirms that the attacker is performing a comprehensive vulnerability assessment against the server.

- Many of the requests target resources under `/bwapp/`, which corresponds to **bWAPP** (Buggy Web Application), a deliberately vulnerable application designed for training and testing security skills.

- Many of the responses from the server result in a `404` error, indicating that the requested resources do not exist suggesting that the Nikto scanner is brute-forcing various file extensions and paths in an attempt to discover sensitive files or resources.

- All requests occur within a narrow time window (12:36:24–12:36:42 on 20/Jun/2021) confirming that the activity is automated.

- The targeted files suggest the attacker is searching for configuration files (`.xml`, `.xsl`), executables (`.exe`, `.bin`), system-related files (`.sys`), etc. which could reveal sensitive information or serve as entry points for privilege escalation or lateral movement.

***Question - Which automated scan tool did the attacker use for web reconnaissance?***

![image](https://github.com/user-attachments/assets/c61d1063-2c4f-42ad-ba8c-831c27341ab4)

```bash
Nikto
```

***Question - After web reconnaissance activity, which technique did the attacker use for
directory listing discovery?***

This is evidence of directory listing discovery from the numerous GET requests targeting different file paths and extensions. The attacker is systematically trying different directories and files to identify vulnerabilities.

![image](https://github.com/user-attachments/assets/4dc8a4bc-2af0-4437-ba3b-e3434fa9c6ef)

```bash
Directory Brute Force
```

***Question - What is the third attack type after directory listing discovery?***

After the directory listing discovery, there were attempts against the login page. This is evident from the repeated GET requests to `/bWAPP/login.php` with possibly different username and password combinations.

![image](https://github.com/user-attachments/assets/96abdc3e-e924-4483-87f3-71a0484f13d9)

```bash
Brute Force
```

***Question - Is the third attack successful?***

```bash
Yes
```

***Question - What is the name of the fourth attack?***

From the log file snippet, the fourth attack can be traced to the following lines:

![image](https://github.com/user-attachments/assets/c6367296-14cf-4ecd-9a39-e4973d872d5d)

The attacker created malicious payloads with system-level commands and appended them to the vulnerable parameter `message`. These commands were processed by the application and executed at the system level.

```bash
Code Injection
```

***Question - What is the first payload for 4th attack?***

The attack begins with a basic test payload (`message=test`) to check the vulnerable parameter.

**Payload:** `%22%22;%20system(%27whoami%27)`

**Decoded Command:** `system('whoami')`

Once confirmed, the attacker escalates to executing commands like `whoami` to verify access.

```bash
whoami
```

***Question - Is there any persistency clue for the victim machine in the log file? If yes, what is the related payload?***

**Payload:** `%22%22;%20system(%27net%20user%20hacker%20asd123!!%20/add%27)`

**Decoded Command:** `system('net user hacker asd123!! /add')`

After gaining access, the attacker uses the `net user` command to create a new user account named `hacker` with the password `asd123!!` and adds it to the system. This establishes persistence on the victim machine.

```bash
%27net%20user%20hacker%20asd123!!%20/add%27
```

![image](https://github.com/user-attachments/assets/5ee21925-e845-41da-b055-7107500c6a94)
