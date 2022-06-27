# **Red vs Blue Assesment, Analysis, and Hardening of a Vulnerable System**

## **Table of Contents**

1. [Network Topology](#network-topology)
   - [Network](#network)
   - [Machines](#machines)
2. [Red Team: Security Assesment](#red-team-security-assesment)
   - [Recon: Describing the Target](#recon-describing-the-target)
   - [Vulnerability Assesment](#vulnerability-assesment)
   - [Exploitation](#exploitation)
3. [Blue Team: Log Analysis and Mitigation Strategies](#blue-team-log-analysis-and-mitigation-strategies)
   - [Log Analysis](#log-analysis)
   - [Proposed Alarms and Mitigation Strategies](#proposed-alarms-and-mitigation-strategies)
 

## **Network Topology**

### Network

Address Range: 192.168.1.0/24

Netmask: 255.255.255.0

Gateway: 192.168.1.1

### Machines

|IPv4         |OS            |Hostname|
|-------------|--------------|--------|
|192.68.1.1   |Windows 10    |Hyper-V |
|192.168.1.90 |Kali (Linux)  |Kali    |
|192.168.1.100|Ubuntu (Linux)|ELK     |
|192.168.1.105|Ubuntu (Linux)|Capstone|

![Network Topology](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Diagrams/Project%202-Updated.drawio.png) 


## **Red Team: Security Assesment**

## **Recon: Describing the Target**

Nmap Identified the following hosts on the network:

|Hostname       |IP Address     |Role of Network|
|---------------|---------------|---------------|
|Hyper-V        | 192.168.1.1   | Hypervisor    |
|Kali           | 192.168.1.90  | Attack Machine|
|ELK            | 192.168.1.100 | Monitoring Machine|
|Captstone      | 192.168.1.105 | Victim Machine|



## **Vulnerability Assesment**

The assesment uncovered the following critical vulnerabilities in the target:

### Port Scanning

&nbsp;&nbsp;&nbsp;Description: Method to determine what ports are open.

&nbsp;&nbsp;&nbsp;Impact: Attackers can discover vulnerabilities associated with each visible open port with a port scan.

### Broken Authentication

 &nbsp;&nbsp;&nbsp;Description: Broken Authentication attacks are used to exploit user accounts. 

 &nbsp;&nbsp;&nbsp;Impact: Attackers that are able to obtain valid credentials are able to have access to password protected directories and files. 

### Sensitive Data Exposure

 &nbsp;&nbsp;&nbsp;Description: Sensitive Data is any information that is meant to be protected from unauthorized access. 

 &nbsp;&nbsp;&nbsp;Impact: If Attackers are able to obtain exposed credentials they will be able to elevate their access to other resources in the domain/server.

### Local File Inclusion

 &nbsp;&nbsp;&nbsp;Description: A Local File Inclusion are attacks that happen when an application includes a file as user input without properly validating it.

 &nbsp;&nbsp;&nbsp;Impact: Attackers are able to trick the web application into running malicious files on the web server.


## **Exploitation**

### Port scanning

### &nbsp;&nbsp;&nbsp;Tools & Processes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Due to open ports being visible during a Nmap scan, I was able to determine the victims IP address as well as the port and service version being used to host a web server.

### &nbsp;&nbsp;&nbsp;Achievements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;I was able to find the IP address of the server and determine that the server was using port 80 to host the website.

![Port Scanning](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/nmap%20scan%20-%20terminal.png)

### Broken Authentication

### &nbsp;&nbsp;&nbsp;Tools & Processes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The tools I used to exploit the web server’s broken authentication vulnerability was by using hydra to perform a brute force attack. The web servers lists the name of the person in charge of system administration. I used their name as the username and used a wordlist to brute force the password with hydra.

### &nbsp;&nbsp;&nbsp;Achievements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The exploited granted access to a restricted secret folder that contained a file describing the process to gain access to the webdav directory. 

![Hydra command](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hydra%20command.png)
![Hydra result](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hydra%20-%20result.png)

### Sensitive Data Explosure

### &nbsp;&nbsp;&nbsp;Tools & Processes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;After gaining access to the file in the restricted secret folder, I found a hash value. I used john the ripper to crack the password.

### &nbsp;&nbsp;&nbsp;Achievements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The exploited granted access to the webdav folder. 

![Data Exposure](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/secret%20folder%20file.png)

### Local File Inclusion

### &nbsp;&nbsp;&nbsp;Tools & Processes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;After gaining access to the webdav server I was able to craft a PHP reverse shell payload using msfvenom and uploaded the payload to the server. I also setup a listener using the metasploit framework in order to catch the reverse shell after being executed.

### &nbsp;&nbsp;&nbsp;Achievements

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The exploit granted access to the web server operating system. 

![Local File Inclusion](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/Webdav.png)


## **Blue Team: Log Analysis and Mitigation Strategies**


## **Log Analysis**

### Identifying the Port Scan

#### &nbsp;&nbsp;&nbsp;What time did the port scan occur?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The port scan occurred at 1:53pm based on the logs.

#### &nbsp;&nbsp;&nbsp;How many packets were sent, and from which IP address?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1,100 packets were sent.

#### &nbsp;&nbsp;&nbsp;What indicates that this was a port scan?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The destination ports for each packet were all different.

![Port scan](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/Nmap%20-T2%20-F%20-sV%20scan.png)

### Finding the Request for the Hidden Directory

#### &nbsp;&nbsp;&nbsp;What time did the request occur? How many request were made?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The request occurred at 2:34pm.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 request was made.

#### &nbsp;&nbsp;&nbsp;Which files were requested? What did they contain?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The file requested was "secret_folder".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It contains the "connect_to_corp_server" file.

![Hidden Directory](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hidden%20directory%20request.png)

### Uncovering the Brute Force Attack

#### &nbsp;&nbsp;&nbsp;How many requests were made in the attack?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;There were 15,606 requests made during the brute force attack.

#### &nbsp;&nbsp;&nbsp;How many requests had been made before the attacker discovered the password?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Based on the logs there were very few requests made before the attack.

![Brute Force](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/brute%20force%20attack.png)

### Finding the WebDAV Connection

#### &nbsp;&nbsp;&nbsp;How many requests were made to this directory?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22 requests were made to the WebDAV directory.

#### &nbsp;&nbsp;&nbsp;Which files were requested?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password.dav and payload.php were requested.


## **Proposed Alarms and Mitigation Strategies**

### Blocking the Port Scan

#### Alarm

#### &nbsp;&nbsp;&nbsp;What kind of alarm can be set to detect future port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We can setup-up an email alert.

#### &nbsp;&nbsp;&nbsp;What threshold would you set to activate this alarm?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The threshold for the alert should be for 10 or more incoming packets that are requesting information about different ports from a single source in under a minute.

#### System Hardening

#### &nbsp;&nbsp;&nbsp;What configurations can be set on the host to mitigate port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Configure the firewall to enable packet filtering to mitigate port scanning. Deny all ICMP requests from external hosts.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup a SIEM platform for monitoring port scan activity in real-time and setup response measures such as alerts for rapid response inf needed

### Finding the Request for the Hidden Directory

#### Alarm

#### &nbsp;&nbsp;&nbsp;What kind of alarm can be set to detect future port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup an email alert.

#### &nbsp;&nbsp;&nbsp;What threshold would you set to activate this alarm?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The threshold of the alert should be set to 3 failed attempts from a single internal IP address. External IP address failed attempts should trigger on each attempt.

#### System Hardening

#### &nbsp;&nbsp;&nbsp;What configurations can be set on the host to mitigate port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Implement error redirection, such as to deny any feedback to attackers by displaying a HTTP response code.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adjust password policy requirements for users that have access to hidden directories.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Encrypt contents within hidden directories.

### Preventing Brute Force Attacks

#### Alarm

#### &nbsp;&nbsp;&nbsp;What kind of alarm can be set to detect future port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup an email alert.

#### &nbsp;&nbsp;&nbsp;What threshold would you set to activate this alarm?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The alert should trigger after 5 consecutive failed attempts.

#### System Hardening

#### &nbsp;&nbsp;&nbsp;What configurations can be set on the host to mitigate port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Limit login attempts by locking the user account for a period of time after 5 consecutive failed attempts.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Use a secure platform session manager to randomly generate session identifiers.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup MFA for all users so in the event a threat actor does acquire valid credentials they are unable to aunthenticate through additional factors required by MFA.

### Detecting the WebDAV Connection

#### Alarm

#### &nbsp;&nbsp;&nbsp;What kind of alarm can be set to detect future port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup an email alert for login attempts and login location.

#### &nbsp;&nbsp;&nbsp;What threshold would you set to activate this alarm?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The alert for failed attempts should trigger after 5 failed attempts. The alert for the login location should trigger every time an uncommon location is detected on login attempt.

#### System Hardening

#### &nbsp;&nbsp;&nbsp;What configurations can be set on the host to mitigate port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Limit login attempts by locking the user account for a period of time after 5 consecutive failed attempts.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Only allow terminals that hold a recognized SSL certificate to connect to the WebDAV. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup MFA for all users so in the event a threat actor does acquire valid credentials they are unable to authenticate through additional factors required by MFA.

### Identifying Reverse Shell Uploads

#### Alarm

#### &nbsp;&nbsp;&nbsp;What kind of alarm can be set to detect future port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup an email alert.

#### &nbsp;&nbsp;&nbsp;What threshold would you set to activate this alarm?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The alert should trigger every time anyone attempts to upload an executable file.

#### System Hardening

#### &nbsp;&nbsp;&nbsp;What configurations can be set on the host to mitigate port scans?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Limit allowed file types to avoid executable files from being uploaded.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup an anti-malware engine such as McAfee Gateway Anti-Malware engine to scan all uploaded files for malware.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Store uploaded files outside of the websites public directory to prevent attackers from executing any uploaded files.
