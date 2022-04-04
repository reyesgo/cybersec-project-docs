## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELK network Diagram](https://github.com/reyesgo/cybersec-project-docs/blob/main/Diagrams/ELK%20Network%20Diagram%20Updated.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

[ELK Playbook file](https://github.com/reyesgo/cybersec-project-docs/tree/main/Ansible/Playbooks)

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

- The use of a Load balancer is to prevent a server from overloading by distributing the flow of traffic across all available servers. Another benefit of a load balancer is that it can run SSL off-loading to repel a DDoS attack by forwarding all SSL based encryption to a dedicated server to decrypt incoming traffic and then forward the unencrypted traffic to the available web servers to relieve them from using its resources to decrypt the data.  
- the Advantage of a jumb box is that it provides another layer in the defense in depth approach. The concept of a jump box is a security hardened single point to our network. It acts as a bridge between two trusted systems in the network.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system. we use the following beats to accomplish this task:

 -  Filebeat monitors the log files and forwards them to ELK server to be indexed.
 -  Metricbeat collects metrics from the operating system and from services running on the server to be analysed.

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
- 23.81.114.222 - personal laptop

Machines within the network can only be accessed by the container in the jump box.
- The ansible container is the only machine that can access all other virtual machines in the network such as the 2 web servers and the ELK server via asymmetric encryption.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 23.81.114.222:22     |
| Web-1    | Yes                 | 23.81.114.222:80     |
| Web-2    | Yes                 | 23.81.114.222:80     |
| ELK      | Yes                 | 23.81.114.222:5601   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- first step is to install docker.io to download the package containing the docker platform to our system. 
- next step we download python3-pip used for installing and managing python images/packages.
- next we install the docker engine that will house all our containers. this is were we will install our ELK image.
- next we will create a rule to allocate the required amount of virtual memory for the ELK stack to perform as intended.
- lastly we download and install the elk image and configure it so that it is enabled on system boot.  

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![screenshot of docker ps output](https://github.com/reyesgo/cybersec-project-docs/blob/main/Images/elk-container.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
| Name  | IP address |
|-------|------------|
| Web-1 | 10.0.0.4   |
| Web-2 | 10.0.0.5   |

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat monitors and collects System changes such as system log events, sudo command events, SSH login attempts, and the creating of new users and groups and forwards them to the ELK server to be analysed via our Kibana web interface. 
- Metricbeat monitors and collects information of the overall health of our docker contained web servers such as system, host and container metrics that we can also analyse via the Kibana web interface.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the host file to /etc/ansible.
- Update the host file to include the IP address of remote hosts.
- Run the playbook, and navigate to the remote host machine to check that the installation worked as expected.

- All files with the .yml extension are our playbooks, they are all located in the Playbook folder in our repository. when copying these files over to a control machine they will need to be stored in the /etc/ansible folder.

- When using the various playbooks the hosts field in the playbook needs to be modified to specify the group of remote hosts to be modified by the commands written within the playbook. Another step to keep in mind is to update the hosts file to specify the group of machines in your environment to be affected by a playbook.   

- Kibana can be accessed through your locak browser by navigating to the ELK servers public IP address through port 5601.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
