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

## Vulnerability Assesment

The assesment uncovered the following critical vulnerabilities in the target:

### Port Scanning

Description: Method to determine what ports are open.

Impact:

### Broken Authentication

Description: Broken Authentication attacks are used to exploit user accounts. 

Impact:

### Sensitive Data Exposure

Description: Sensitive Data is any information that is meant to be protected from unauthorized access. 

Impact:

### Local File Inclusion

Description: A Local File Inclusion are attacks that happen when an application includes a file as user input without properly validating it.

Impact:

## Blue Team: Log Analysis and Attack Characterization

## Hardening: Proposed Alarms and Mitigation Strategies
