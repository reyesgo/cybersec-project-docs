# Blue Team: Summary of Operations

## Table of Contents

1. [Network Topology](#network-topology)
2. [Description of Targets](#description-of-targets)
3. [Monitoring the Targets](#monitoring-the-targets)
4. [Suggestions for Going Further](#suggestions-for-going-further)

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

![Excessive HTTP errors image]()

- ### HTTP Request Size Monitor

The HTTP request size monitor alert is implemented as follows:

- Metric: HTTP request bytes
- Threshold: Above 3500
- Vulnerability Mitigated: DDoS and Code Injection in HTTP request (XSS). 
- Reliability: Low reliability, the alert is prone to frequent false positives as there are non-malicous requests above 3500 bytes as we can see from the image below. This alert will need more testing in order to determine the correct threshold in order for it to be more reliable.

![HTTP request size image]()

- ### CPU Usage Monitor

The CPU usage monitor alert is implemented as follows:

- Metric: Total amount of system CPU usage by processes
- Threshold: above 0.5 (50%)
- Vulnerability Mitigated: Malicious software/programs using CPU resources.
- Reliability: High reliability, measuring by CPU usage allows us to determine if malware is attempting to compromise the system.

![CPU Usage image]()

## Suggestions for Going Further
