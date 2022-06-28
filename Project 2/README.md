# **Red vs Blue Assessment, Analysis, and Hardening of a Vulnerable System**

## **Table of Contents**

1. [Network Topology](#network-topology)
   - [Network](#network)
   - [Hosts](#hosts)
2. [Red Team: Security Assessment](#red-team-security-assessment)
   - [Recon: Describing the Target](#recon-describing-the-target)
   - [Vulnerability Assessment](#vulnerability-assessment)
   - [Exploitation](#exploitation)
3. [Blue Team: Log Analysis and Mitigation Strategies](#blue-team-log-analysis-and-mitigation-strategies)
   - [Log Analysis](#log-analysis)
   - [Proposed Alarms and Mitigation Strategies](#proposed-alarms-and-mitigation-strategies)
 

## **Network Topology**

### Network

Address Range: 192.168.1.0/24

Netmask: 255.255.255.0

Gateway: 192.168.1.1

### Hosts

|IPv4         |OS            |Hostname|
|-------------|--------------|--------|
|192.68.1.1   |Windows 10    |Hyper-V |
|192.168.1.90 |Kali (Linux)  |Kali    |
|192.168.1.100|Ubuntu (Linux)|ELK     |
|192.168.1.105|Ubuntu (Linux)|Capstone|

![Network Topology](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Diagrams/Project%202-Updated.drawio.png) 


## **Red Team: Security Assessment**

## **Recon: Describing the Target**

Nmap Identified the following hosts on the network:

|Hostname       |IP Address     |Role of Network|
|---------------|---------------|---------------|
|Hyper-V        | 192.168.1.1   | Hypervisor    |
|Kali           | 192.168.1.90  | Attack Machine|
|ELK            | 192.168.1.100 | Monitoring Machine|
|Capstone       | 192.168.1.105 | Victim Machine|



## **Vulnerability Assessment**

The assessment uncovered the following critical vulnerabilities in the target:

### Port Scanning

- Description: Method to determine what ports are open.

- Impact: Attackers can discover vulnerabilities associated with each visible open port with a port scan.

### Broken Authentication

- Description: Broken Authentication attacks are used to exploit user accounts. 

- Impact: Attackers that can obtain valid credentials can have access to password-protected directories and files. 

### Sensitive Data Exposure

- Description: Sensitive Data is any information that is meant to be protected from unauthorized access. 

- Impact: If Attackers can obtain exposed credentials they will be able to elevate their access to other resources in the domain/server.

### Local File Inclusion

- Description: Local File Inclusion is an attack that happens when an application includes a file as user input without properly validating it.

- Impact: Attackers can trick the web application into running malicious files on the web server.


## **Exploitation**

- ### Port scanning

   - ### Tools & Processes

      - Due to open ports being visible during a Nmap scan, I was able to determine the victim's IP address as well as the port and service version being used to host a web server.

   - ### Achievements

      - I was able to find the IP address of the server and determine that the server was using port 80 to host the website.

![Port Scanning](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/nmap%20scan%20-%20terminal.png)

- ### Broken Authentication

   - ### Tools & Processes

      - The tool I used to exploit the web server’s broken authentication vulnerability was by using hydra to perform a brute force attack. The web servers list the name of the person in charge of system administration. I used their name as the username and used a wordlist to brute force the password with hydra.

   - ### Achievements

      - The exploited granted access to a restricted secret folder that contained a file describing the process to gain access to the WebDAV directory. 

![Hydra command](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hydra%20command.png)
![Hydra result](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hydra%20-%20result.png)

- ### Sensitive Data Exposure

   - ### Tools & Processes

      - After gaining access to the file in the restricted secret folder, I found a hash value. I used john the ripper to crack the password.

   - ### Achievements

      - The exploited granted access to the WebDAV folder. 

![Data Exposure](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/secret%20folder%20file.png)

- ### Local File Inclusion

   - ### Tools & Processes

      - After gaining access to the WebDAV server I was able to craft a PHP reverse shell payload using msfvenom and uploaded the payload to the server. I also set up a listener using the Metasploit framework to catch the reverse shell after being executed.

   - ### Achievements

      - The exploit granted access to the web server operating system. 

![Local File Inclusion](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/Webdav.png)


## **Blue Team: Log Analysis and Mitigation Strategies**


## **Log Analysis**

- ### Identifying the Port Scan

   - #### What time did the port scan occur?

     - The port scan occurred at 1:53 pm based on the logs.

   - #### How many packets were sent, and from which IP address?

      - 1,100 packets were sent.

   - #### What indicates that this was a port scan?

      - The destination ports for each packet were all different.

![Port scan](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/Nmap%20-T2%20-F%20-sV%20scan.png)

- ### Finding the Request for the Hidden Directory

   - #### What time did the request occur? How many requests were made?

      - The request occurred at 2:34 pm.

      - 1 request was made.

   - #### Which files were requested? What did they contain?

      - The file requested was "secret_folder".

      - It contains the "connect_to_corp_server" file.

![Hidden Directory](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/hidden%20directory%20request.png)

- ### Uncovering the Brute Force Attack

    - #### How many requests were made during the attack?

      - There were 15,606 requests made during the brute force attack.

    - #### How many requests had been made before the attacker discovered the password?

      - Based on the logs there were very few requests made before the attack.

![Brute Force](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%202/Images/brute%20force%20attack.png)

- ### Finding the WebDAV Connection

   - #### How many requests were made to this directory?

      - 22 requests were made to the WebDAV directory.

   - #### Which files were requested?

      - Password.dav and payload.php were requested.


## **Proposed Alarms and Mitigation Strategies**

- ### Blocking the Port Scan

   - ### Alarm

    - #### What kind of alarm can be set to detect future port scans?

      - We can set up an email alert.

    - #### What threshold would you set to activate this alarm?

      - The threshold for the alert should be for 10 or more incoming packets that are requesting information about different ports from a single source in under a minute.

   - ### System Hardening

   - #### What configurations can be set on the host to mitigate port scans?

      - Configure the firewall to enable packet filtering to mitigate port scanning. Deny all ICMP requests from external hosts.

      - Setup a SIEM platform for monitoring port scan activity in real-time and set up response measures such as alerts for rapid response if needed.

 - ### Finding the Request for the Hidden Directory
   
   - ### Alarm   

   - #### What kind of alarm can be set to detect future port scans?

      - Set up an email alert.

   - #### What threshold would you set to activate this alarm?

      - The threshold of the alert should be set to 3 failed attempts from a single internal IP address. External IP address failed attempts should trigger on each attempt.

   - ### System Hardening

   - #### What configurations can be set on the host to mitigate port scans?

      - Implement error redirection, such as denying any feedback to attackers by displaying an HTTP response code.

      - Adjust password policy requirements for users that have access to hidden directories.

      - Encrypt contents within hidden directories.

- ### Preventing Brute Force Attacks

   - ### Alarm

   - #### What kind of alarm can be set to detect future port scans?

      - Set up an email alert.

   - #### What threshold would you set to activate this alarm?

      - The alert should trigger after 5 consecutive failed attempts.

   - ### System Hardening

   - #### What configurations can be set on the host to mitigate port scans?

      - Limit login attempts by locking the user account for some time after 5 consecutive failed attempts.

      - Use a secure platform session manager to randomly generate session identifiers.

      - Setup MFA for all users so in the event a threat actor does acquire valid credentials they are unable to authenticate through additional factors required by MFA.

- ### Detecting the WebDAV Connection

   - ### Alarm

   - #### What kind of alarm can be set to detect future port scans?

      - Set up an email alert for login attempts and log-in location.

   - #### What threshold would you set to activate this alarm?

      - The alert for failed attempts should trigger after 5 failed attempts. The alert for the login location should trigger every time an uncommon location is detected on log-in attempt.

   - ### System Hardening

   - #### What configurations can be set on the host to mitigate port scans?

      - Limit login attempts by locking the user account for some time after 5 consecutive failed attempts.

      - Only allow terminals that hold a recognized SSL certificate to connect to the WebDAV. 

      - Setup MFA for all users so in the event a threat actor does acquire valid credentials they are unable to authenticate through additional factors required by MFA.

- ### Identifying Reverse Shell Uploads

   - ### Alarm

   - #### What kind of alarm can be set to detect future port scans?

      - Set up an email alert.

   - #### What threshold would you set to activate this alarm?

      - The alert should trigger every time anyone attempts to upload an executable file.

   - ### System Hardening

   - #### What configurations can be set on the host to mitigate port scans?

      - Limit allowed file types to avoid executable files from being uploaded.

      - Setup an anti-malware engine such as McAfee Gateway Anti-Malware engine to scan all uploaded files for malware.

      - Store uploaded files outside of the website's public directory to prevent attackers from executing any uploaded files.
