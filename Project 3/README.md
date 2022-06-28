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

## Network Topology

The following machines wre identified on the network:

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
- Reliability: High reliability, when we measure by HTTP response status code we are able to filter possible errors. Response codes between 400 and 599 are client and server error codes. A response code within this range could indicate an attack.

![Excessive HTTP errors image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/Excessive%20HTTP%20Errors%20-%20alert%20history.png)

- ### HTTP Request Size Monitor

The HTTP request size monitor alert is implemented as follows:

- Metric: HTTP request bytes
- Threshold: Above 3500
- Vulnerability Mitigated: DDoS and Code Injection in HTTP request (XSS). 
- Reliability: Low reliability, the alert is prone to frequent false positives as there are non-malicous requests above 3500 bytes as we can see from the image below. This alert will need more testing in order to determine the correct threshold in order for it to be more reliable.

![HTTP request size image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/HTTP%20Request%20Size%20Monitor%20-%20alert%20history.png)

- ### CPU Usage Monitor

The CPU usage monitor alert is implemented as follows:

- Metric: Total amount of system CPU usage by processes
- Threshold: above 0.5 (50%)
- Vulnerability Mitigated: Malicious software/programs using CPU resources.
- Reliability: High reliability, measuring by CPU usage allows us to determine if malware is attempting to compromise the system.

![CPU Usage image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Blue/CPU%20Usage%20Monitor%20-%20alert%20history.png)

## Suggestions for Going Further

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

- WordPress user enumeration
   - Patch: WordPress information obfuscation
   - Why it Works: Remove or disable files containing sensitive information can slow down attackers when attempting to gather information regarding the platform. A few of the files that will need to be re-configured are:
     - xmlrpc.php
     - ?authors=
     - readme.html
     - wp-cron.php  

- Vulnerable SSH Authentication
   - Patch: MFA implementation
   - Why it Works: MFA prevents brute force attacks by requiring a second authentication factor in order to successfully use login credentials.

- Server Sensitive Data Exposure
   - Patch: Restric access to wp-config.php file
   - Why it Works: the file has a default location by moving the file to a file outside of the wordpress installation and restricting file permissions we can ensure it is not accessed by unauthorized users.

- MySQL Access
   - Patch: Implement encryption protocol/MFA
   - Why it Works: By using encryption protocols, data breaches are less likely to happen. Cybercriminals are therefore unable to access the data even if they get their hands on it. multi-factor authentication will also prevent any unauthorized user from access the database directly.

# Red Team: Summary of Operations

## Exposed Services

Nmap scan results for the target machine reveal the below services and OS details:

![nmap scan image](https://github.com/reyesgo/cybersec-project-docs/blob/main/Project%203/Images/Red/nmap%20-Pn%20-sV%20192.168.1.110.png)

This scan identifies the services belwo as potential points of entry:

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
     - Used
       
