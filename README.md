## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELK network Diagram](https://github.com/reyesgo/cybersec-project-docs/blob/main/Diagrams/ELK%20Network%20Diagram%20Updated.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

- [DVWA Playbook](https://github.com/reyesgo/cybersec-project-docs/tree/main/Ansible/Playbooks/DVWA)
- [ELK Playbook](https://github.com/reyesgo/cybersec-project-docs/tree/main/Ansible/Playbooks/ELK)
- [Filebeat Playbook](https://github.com/reyesgo/cybersec-project-docs/tree/main/Ansible/Playbooks/Filebeat)
- [Metricbeat Playbook](https://github.com/reyesgo/cybersec-project-docs/tree/main/Ansible/Playbooks/Metricbeat)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network. 

- A load balancer guard against a single server overloading by distributing the flow of traffic across all available servers. it monitors the health of its registered targets and routes traffic only to healthy targets. 
  
- A jumb box provides another layer in the defense in depth approach. The concept of a jump box is a security hardened single point to our network. It acts as a bridge between two trusted systems in the network.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system. we use the following beats to accomplish this task:

 -  Filebeat monitors the log files and forwards any changes to the ELK server.
 -  Metricbeat collects metrics and system resources usage from a host and sends to the ELK server.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | DVWA     | 10.0.0.5   | Linux            |
| Web-2    | DVWA     | 10.0.0.6   | Linux            |
| ELK      | ELK      | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 23.81.114.222

Machines within the network can only be accessed by the ansible container in the jump box.

- Ansible containter
  - Private IP: 172.17.0.2 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 23.81.114.222:22     |
| Web-1    | Yes                 | 23.81.114.222:80     |
| Web-2    | Yes                 | 23.81.114.222:80     |
| ELK      | Yes                 | 23.81.114.222:5601   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it increases efficiency and allows us to quickly scale our environment through the use of playbooks. For example, if we want to install an updated version of a specific type of software to a group of machines in our enviroment. We can accomplish this by creating a new host group in the hosts file, writing out all the IP addresses of the machines that need to be updated under that group and create a playbook to install the update to all machines in the group through the playbook. 

The playbook implements the following tasks:
- Install docker.io: downloads the package containing the docker platform to our system. 
- Install python3-pip: installs a python packaging management system used to install and manage software images/packages.
- Install docker module: installs the docker engine that will house all our containers. this is were we will install our ELK image.
- Increase virtual memory/Use more memory: creates a rule to allocate the required amount of virtual memory for the ELK stack to perform as intended.
- Download and launch a docker elk container: downloads, configures and installs the elk image.  
- Enable service docker on boot: enables the docker engine on system boot.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![screenshot of docker ps output](https://github.com/reyesgo/cybersec-project-docs/blob/main/Images/elk-container-updated.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name     | IP address |
|----------|------------|
| Jump Box | 10.0.0.4   |
| Web-1    | 10.0.0.5   |
| Web-2    | 10.0.0.6   |

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat monitors and collects System changes such as system log events, sudo command events, SSH login attempts, and the creating of new users and groups and forwards them to the ELK server to be analysed via our Kibana web interface.
 
- Metricbeat monitors and collects information of the overall health of our docker contained web servers such as system, host and container metrics that we can also analyse via the Kibana web interface.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the install-elk.yml to /etc/ansible by running the following command.
  - curl https://raw.githubusercontent.com/reyesgo/cybersec-project-docs/main/Ansible/Playbooks/ELK/install-elk.yml > /etc/ansible/install-elk.yml

- Update the host file in /etc/ansible by creating a host group and include the IP address of the host that will function as the ELK server.
  
  - ![screenshot of elk-ip example](https://github.com/reyesgo/cybersec-project-docs/blob/main/Images/elk-ip-address.png)
  - [elk] = the host group
  - x.x.x.x = the IP address of your ELK server

- Run the playbook. 
  - ansible-playbook /etc/ansible/install-elk.tml 
 
- Navigate to http://[ELK-server-public-ip-address]:5601 to check that the installation worked as expected.

