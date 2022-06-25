# Red vs Blue Assesment, Analysis, and Hardening of a Vulnerable System

## Table of Contents

1. [Network Topology](#network-topology)
2. [Red Team: Security Assesment](#red-team-security-assesment)
3. [Blue Team: Log Analysis and Attack Characterization](#blue-team-log-analysis-and-attack-characterization)
4. [Hardening: Proposed Alarms and Mitigation Strategies](#hardening-proposed-alarms-and-mitigation-strategies)

## Network Topology

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


## Red Team: Security Assesment

### Recon: Describing the Target

Nmap Identified the following hosts on the network:

|Hostname       |IP Address     |Role of Network|
|---------------|---------------|---------------|
|Hyper-V        | 192.168.1.1   | Hypervisor    |
|Kali           | 192.168.1.90  | Attack Machine|
|ELK            | 192.168.1.100 | Monitoring Machine|
|Captstone      | 192.168.1.105 | Victim Machine|

---

### Vulnerability Assesment

The assesment uncovered the following critical vulnerabilities in the target:

#### Port Scanning

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description: Method to determine what ports are open.

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Impact: Attackers can discover vulnerabilities associated with each visible open port with a port scan.

#### Broken Authentication

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description: Broken Authentication attacks are used to exploit user accounts. 

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Impact: Attackers that are able to obtain valid credentials are able to have access to password protected directories and files. 

#### Sensitive Data Exposure

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description: Sensitive Data is any information that is meant to be protected from unauthorized access. 

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Impact: If Attackers are able to obtain exposed credentials they will be able to elevate their access to other resources in the domain/server.

#### Local File Inclusion

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description: A Local File Inclusion are attacks that happen when an application includes a file as user input without properly validating it.

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Impact: Attackers are able to trick the web application into running malicious files on the web server.

---

### Exploitation

#### Port scanning

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tools & Processes

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Achievements

#### Broken Authentication

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tools & Processes

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Achievements

#### Sensitive Data Explosure

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tools & Processes

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Achievements

#### Local File Inclusion

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tools & Processes

##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Achievements

## Blue Team: Log Analysis and Attack Characterization



## Hardening: Proposed Alarms and Mitigation Strategies
