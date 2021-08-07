# JW-ELK-Stack
This is a repository of how to setup and configure an ELK stack.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Cloud Network with ELK Topology](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Diagrams/JW_Unit_13_Cloud_Security_ELK.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

- [DVWA Playbook](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/dvwa_playbook.png)
- [ELK Playbook](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/elk_playbook.PNG)
- [Filebeat Playbook](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/filebeat_playbook.png)
- [Metricbeat Playbook](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/metricbeat_playbook.png)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secure, in addition to restricting access to the network.
- The Load Balencer provide a higher level of security by defending against distributed denial-of-service (DDoS) attacks by shifting attack traffic from the cloud network to the cloud provider
- The Jump Box creates a secure and monitored pathway for administrators to manage devices within a network.

_What aspect of security do load balancers protect?_
- Load balancers distribute network traffic across mulitple servers to ensure no one server takes to much demand.  The security function of the load balancer defends against DDoS by shifting attacks away from the network.

_What is the advantage of a jump box?_
- Allows administrators a secure pathway to configure network devices within a sperate set of security rules.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

_What does Filebeat watch for?_  
- Filebeat monitors log files based on specified rules, collects the logs, and sends to Elasticsearch or Logstash. 

_What does Metricbeat record?_ 
- Metricbeat collects metrics from an OS and running services based on specified rules, and sends to Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function   | IP Address | Operating System |
|----------------------|------------|------------|------------------|
| Jump-Box-Provisioner | Gateway    | 10.0.0.8   | Ubuntu 20.04     |
| Web-1                | Web Server | 10.0.0.7   | Ubuntu 20.04     |
| Web-2                | Web Server | 10.0.0.6   | Ubuntu 20.04     |
| ELKSever             | ELK Server | 10.1.0.4   | Ubuntu 20.04     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the DVWA machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

_Add whitelisted IP addresses_
- 134.215.218.43 <client public IP>

Machines within the network can only be accessed by SSH via Jump Box.

_Which machine did you allow to access your ELK VM?_
- Jump Box Provisioner

_What was its IP address?_
- 10.0.0.8

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses    |
|----------------------|---------------------|-------------------------|
| Jump-Box-Provisioner | No                  | 134.215.218.43          |
| Web-1                | Yes                 | 10.0.0.8 134.215.218.43 |
| Web-2                | Yes                 | 10.0.0.8 134.215.218.43 |
| ELKSever             | No                  | 10.0.0.8                |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
_What is the main advantage of automating configuration with Ansible?_
- Network administrators can automatically configure multiple machines at the same time using an ansible playbook.

The playbook implements the following tasks for the ELK Docker:
_In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Direct Ansible ELK
- Download docker.io
- Install phython3-pip
- Install the docker module that was downloaded above
- Increase virtual memory and use that memory as it requires additional memory to run properly
- Download and launch the docker elk container and list the ports that elk runs on
- Enable the docker service on boot to ensure it continues to run on restart

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

- [Docker PS Output](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/ELK_Docker_PS_Output.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
_List the IP addresses of the machines you are monitoring_
- Web-1 10.0.0.7
- Web-2 10.0.0.6
- ELK 10.1.0.4

We have installed the following Beats on these machines:
_Specify which Beats you successfully installed_
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
_In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Filebeats monitors log files based on specified rules, collects the logs, and sends to Elasticsearch or Logstash for indexing.
  - [Kibana Log Exxample](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/Kibana_Logs.PNG)
- Metricbeats collects metrics from an OS and running services based on specified rules, and sends to Elasticsearch or Logstash.
  - [Kibana Metric Example](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/Kibana_Metrics.PNG)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YAML file to /etc/ansible.
- Update the hosts file to include target VM IP addresses
- Run the playbook, and navigate to web page to check that the installation worked as expected.

_Answer the following questions to fill in the blanks:_
- _Which file is the playbook?_ YAML or .yml 
- _Where do you copy it?_ /etc/ansible
- _Which file do you update to make Ansible run the playbook on a specific machine?_ hosts
- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ Configure hosts to direct ELK to the ELK private IP address and direct - - Filebeats to the webserver IP address.
  - [Ansible Hosts Configuration](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Images/Ansible_Hosts_Config.PNG)
- _Which URL do you navigate to in order to check that the ELK server is running?_ http://[your.ELK-VM.External.IP]:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

1. #ssh jump_box_provisioner
2. ssh -i ~/.ssh/id_rsa azdmin@<public_host_ip>
3.
4. #docker setup
5. sudo apt install docker.io
6. sudo docker pull cyberxsecurity/ubuntu.bionic
7. sudo docker run ti cyberxsecurity/ubuntu:bionic bash
8. sudo docker run -ti cyberxsecurity/ansible:latest bash
9.
10. #run docker
11. sudo docker container list -a
12. sudo docker start <container_name>
13. sudo docker attach <container_name>
14.
15. #ssh container
16. ansible webservers -m print
17. ssh azdmin@<container_private_ip>
