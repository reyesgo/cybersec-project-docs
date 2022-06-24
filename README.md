## Project 1 - ELK STACK

### Description

Setup a cloud monitoring system by configuring an ELK Stack Server in a Microsoft Azure environment. The server was used to monitor a vulnerable virtual machine using DVWA for any changes to the system. Setup Filebeat and Metricbeat to monitor and collect data from the vulnerable machine on the network. Created playbooks for the DVWA (D*mn vulnerable web application) and ELK stack and its Beats suite. 
 
### Objectives

- Created and configured virtual network.
- Created and configured virtual machines in a cloud network.
- Created Ansible playbook to install and configure ELK server.
- Installed and launched Docker containers to virtual machines.
- Created Ansible playbook to install and configure Filebeat and Metricbeat.
- Created a diagram of the network and documented the process.

### Skills/Tools Used

- Linux
- Azure
- Ansible
- Docker
- ELK
  - Elasticsearch
  - Logstash
  - Kibana
  - Filebeat
  - Packetbeat
  - Metricbeat

## Project 2 - Red vs Blue Part 1

### Description

Played the role of both penetration tester and SOC (Security operations center) analyst. As the Red Team, I attacked a vulnerable virtual machine within the environment, ultimately gaining root access to the machine. As the Blue team, I used Kibana to review logs to extract data taken during the penetration test to create visualizations in Kibana. Interpreted extracted data to suggest mitigation measures for each exploit in an Assessment report. 

### Objectives

- Enumerated, exploited, and escalated privileges through vulnerable network services(WebDAV) on a virtual machine.
- Recorded penetration testing actions using an ELK server.
- Analyzed logs to determine attack techniques.
- Created a diagram of the network.
- Summarized Red and Blue team findings in an assessment report.
- Proposed hardening and mitigation strategies.

### Skills/Tools Used

- Linux
- Nmap
- dirb
- Hydra
- John the Ripper
- Metasploit
- curl
- MSVenom
- ELK
  - Elasticsearch
  - Logstash
  - Kibana
  - Filebeat
  - Packetbeat
  - Metricbeat

## Project 3 - Red vs Blue Part 2/Network Forensics

### Description

This project is a continuation of Project 2. As the Red Team, I attacked a different vulnerable virtual machine within the same environment to gain root access to the machine to capture flags. AS the Blue team, I implemented mitigation strategies proposed in the previous project in the form of alerts before performing the Red Team objectives to see dashboard alerts in real-time, in addition. I used Wireshark to capture and analyze live traffic to detect several suspicious activities on the virtual network. 

### Objectives

Red vs Blue team Objectives

- Configured alerts proposed in Kibana from Project 2.
- Enumerated, exploited, and escalated privileges through a vulnerable web server using WordpPress on a virtual machine.
- Captured flags on the victim machine.
- Created and submitted an offensive red team analysis report.
- Created and submitted a defensive blue team analysis report.

Network Forensics Objectives

- Captured and analyzed network traffic using Wireshark.
- Inspected network traffic to find evidence of corporate network misuse. 
- Inspected network traffic to find evidence of a malware attack.
- Created and submitted network forensic analysis report.

### Skills/Tools Used

- Linux
- Nmap
- wpscan
- Hydra
- John the Ripper
- MySQL
- Wireshark
- ELK
  - Elasticsearch
  - Logstash
  - Kibana
  - Filebeat
  - Packetbeat
  - Metricbeat
