# Rickys-ASU-Cybersecurity-Bootcamp-Project1
Arizona State University Cybersecurity Bootcamp ELK Stack Project

## Automated ELK Stack Deployment

Task: Build a log management platform using the open-source Elasticsearch, LogStash, Kibana ("ELK") Stack tools.

Criteria:

  - Set up a virtual private cloud network.
  - Protect the cloud network with a firewall.
  - Deploy a virtual computer(s) to the cloud network that will be a JumpBox and Provisioner.
  - Access the entire VNet from the jump box.
  - Install and run containers using Docker.
  - Set up Ansible connections to VMs inside the VNet.
  - Write Ansible playbooks to configure VMs.
  - Create a load balancer on the Azure platform.
  - Create a firewall and load balancer rules to allow traffic to the correct virtual machines.
  - Verify redundancy by turning off one or more virtual machines used in the infrastructure.
  - Deploying containers using Ansible and Docker.
  - Deploying Filebeat using Ansible.
  - Deploying the ELK stack on a server.
  - Diagramming networks and creating a README.

Summary of the ELK Stack: The ELK Stack is comprised of ElasticSearch, LogStash, and Kibana. ElasticSearch is used for collecting and centrally storing logs, LogStash is used for indexing, processing, and storing logs, and Kibana is a web interface used for visualizing the logged data in real-time. Centralizing log data allows for easy searches, early detection, and isolation of IT issues, as well as troubleshooting applications and servers. In more advanced instances, ELK stack can be used for web analytics and data that provides actionable information for a business. In addition, Beats will be added to the ELK stack. Beats allow us to collect the various information. In particular, Filebeat monitors logs files of a machine and ships that log data, in order to centralize the information. This log data can include logons, or other file system information. While, Metricbeat is set up to ship machine metrics like CPU usage, memory, and file system information, to a centralized location.


The files in this repository were used to configure the network depicted below.

![](/images/Red-Team-Network.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook (.yml) file may be used to install only certain pieces of it, such as Filebeat.

  - [my-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/my-playbook.yml) was used to install the web servers
  - [elk-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/elk-playbook.yml) was used to install the ELK server
    - [filebeat-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/filebeat-playbook.yml) was used to install and configure Filebeat on the ELK and Web servers
    - [metricbeat-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/metricbeat-playbook.yml) was used to install and configure Metricbeat on the ELK and Web servers

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unwanted access to the network.
By ensuring availability of the web servers, load balancing is protecting the availability aspect of security in regards to the CIA triad.

A Jump Box was created to make the administtation tasks of the network easier. The main advantage of using a Jump Box is having a single origination point for administrative tasks. The Jump Box ultimately makes the network more secure as well as it only allows access to those who have been granted access. In order to perform administrative tasks it is necessary to access the Jump Box before the other servers can be accessed.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
  - Filebeat scans for log files and locations and collects log events.
  - Metricbeat records statistical data and metrics from the OS and from other services that are also running on the server.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System        |
|----------|----------|------------|-------------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux (Ubuntu 18.04 LTS)|
| Web-1    | Webserver| 10.1.0.8   | Linux (Ubuntu 18.04 LTS)|
| Web-2    | Webserver| 10.1.0.9   | Linux (Ubuntu 18.04 LTS)|
| ELK-VM1  | ELK stack| 10.2.0.4   | Linux (Ubuntu 18.04 LTS)|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Address
 
Machines within the network can only be accessed by SSH.
- The ELK stack machine can only be accessed via SSH 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible  |   Allowed IP Addresses   |
|----------|----------------------|--------------------------|
| Jump Box |   No                 | Personal IP Address      |
|  Web-1   |Yes, via Load Balancer| 104.45.221.188 (JumpBox) |
|  Web-2   |Yes, via Load Balancer| 104.45.221.188 (JumpBox) |
|ELK-Server|   No                 | Personal IP via port 5601|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because 

The playbook implements the following tasks:
1. Install pip3 and Docker.io
2. Download and configure the elk docker container
3. Increase Virtual Machine memory

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](/images/docker%20ps.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.1.0.8
- Web-2 10.1.0.9

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat watches for log files and locations and collects log events. 
- Metricbeat records statistical data and metrics from the OS and from the services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansilbe/files
- Update the configuration file to include the private IP address of the ELK server to the ElasticSearch and Kibana sections of the file.
- Run the playbook, and navigate to (ELK-server-publicIP):5601/app/kibana to check that the installation worked as expected.

- The [elk-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/elk-playbook.yml) is used to install the ELK server. The [filebeat.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/filebeat-playbook.yml) server is used to install and configure Filebeat on the ELK server and web servers. The [metricbeat-playbook.yml](https://github.com/Rickyquintanar/Rickys-ASU-Cybersecurity-Bootcamp-Project1/blob/main/Ansible/metricbeat-playbook.yml) is used to install and configure Metricbeat on the ELK and web servers. The files are copied to the /etc/ansible folder.
- To make Ansible run the playbook on a spefic machine it is necessary to update the /etc/ansible/hosts file and add the private IP address of the machine that you want ansilbe to run the playbook on. 
- In order to check that the ELK server is running correctly it is necessary to navigate to http://(ELK-server-public-IP):5601/app/kibana

- To run the Ansible Configuration of the ELK server and test it the following commands should be run:

1. ssh azadmin@(public IP of Jumpbox)
2. sudo docker container list -a (This will display the available ansible container)
3. sudo docker start (name of ansible container)
4. sudo docker attach (name of ansible container)  -this should drop you into a root shell of the container
5. cd /etc/ansible  -this is where the ansible playbooks are contained
6. ansible-playbook elk-playbook.yml  -this will install and configure the ELK server
7. cd /etc/ansible/files -this is where the metricbeat and filebeat playbooks are contained
8. ansible-playbook metricbeat-playbook.yml  -this will install and configure metricbeat
9. ansible-playbook filebeat-playbook.yml  -this will install and configue filebeat
10. Open a web browser on you local workstation and navigate to http://(ELK-server-public-IP):5601/app/kibana 
