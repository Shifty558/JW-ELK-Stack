# JW-ELK-Stack
This is a repository of how to setup and configure an ELK stack.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Cloud Network with ELK Topology](https://github.com/Shifty558/JW-ELK-Stack/blob/main/Diagrams/JW_Unit_13_Cloud_Security_ELK.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  [DVWA Playbook](Images/dvwa_playbook.png)
  [ELK Playbook](Images/elk_playbook.png)
  [Filebeat Playbook](Images/filebeat_playbook.png)
  [Metricbeat Playbook](Images/metricbeat_playbook.png)

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
- Load Balencer provide a higher level of security by defending against distributed denial-of-service (DDoS) attacks by shifting attack traffic from the cloud network to the cloud provider
- The Jump Box creates a secure and monitored pathway for administrators to manage devices within a network.

What aspect of security do load balancers protect?
- Load balancers distribute network traffic across mulitple servers to ensure no one server takes to much demand.  The security function of the load balancer defends against DDoS by shifting attacks away from the network.

What is the advantage of a jump box?
- Allows administrators a secure pathway to configure network devices within a sperate set of security rules.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- What does Filebeat watch for?  Filebeats monitors log files based on specified rules, collects the logs, and sends to Elasticsearch or Logstash. 
- What does Metricbeat record? Metricbeats collects metrics from an OS and running services based on specified rules, and sends to Elasticsearch or Logstash.

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
- 134.215.218.43

Machines within the network can only be accessed by SSH via Jump Box.
- Which machine did you allow to access your ELK VM? azdmin
- What was its IP address? 134.215.218.43

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses    |
|----------------------|---------------------|-------------------------|
| Jump-Box-Provisioner | No                  | 134.215.218.43          |
| Web-1                | Yes                 | 10.0.0.8 134.215.218.43 |
| Web-2                | Yes                 | 10.0.0.8 134.215.218.43 |
| ELKSever             | No                  | 134.215.218.43          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Network administrators can automatically configure multiple machines at the same time using an ansible playbook.

The playbook implements the following tasks for the ELK Docker:
- Direct Ansible ELK
- Download docker.io
- Install phython3-pip
- Install the docker module that was downloaded above
- Increase virtual memory and use that memory as it requires additional memory to run properly
- Download and launch the docker elk container and list the ports that elk runs on
- Enable the docker service on boot to ensure it continues to run on restart

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

  [Dokcer PS Output](Images/Docker_PS_Output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring
  - Web-1 10.0.0.7
  - Web-2 10.0.0.6
  - ELK 10.1.0.4

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed
  - Filebeats
  - Metricbeats

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
  - Filebeats monitors log files based on specified rules, collects the logs, and sends to Elasticsearch or Logstash for indexing.
    [Kibana Log Exxample](Images/Kibana_Logs.png)
  - Metricbeats collects metrics from an OS and running services based on specified rules, and sends to Elasticsearch or Logstash.
    [Kibana Metric Example](Images/Kibana_Metrics.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YAML file to /etc/ansible.
- Update the hosts file to include target VM IP addresses
- Run the playbook, and navigate to web page to check that the installation worked as expected.

Answer the following questions to fill in the blanks:
- Which file is the playbook? YAML or .yml 
- Where do you copy it? /etc/ansible
- Which file do you update to make Ansible run the playbook on a specific machine? hosts
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? Configure hosts to direct ELK to the ELK private IP address and direct Filebeats to the webserver IP address.
  [Ansible Hosts Configuration](Images/Ansible_Hosts_Config.png)
- Which URL do you navigate to in order to check that the ELK server is running? [Link to ELK Kibana Web Page](http://[your.ELK-VM.External.IP]:5601/app/kibana)

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

#ssh jump_box_provisioner
ssh -i ~/.ssh/id_rsa azdmin@<public_host_ip>

#docker setup
sudo apt install docker.io
sudo docker pull cyberxsecurity/ubuntu.bionic
sudo docker run ti cyberxsecurity/ubuntu:bionic bash
sudo docker run -ti cyberxsecurity/ansible:latest bash

#run docker
sudo docker container list -a
sudo docker start <container_name>
sudo docker attach <container_name>

#ssh container
ansible webservers -m print
ssh azdmin@<container_private_ip>
