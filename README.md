Azure Cloud Network:
Here, you will find a collection of MacOS, Linux, and Ansible scripts used for the Cyber Security Boot Camp.
Scrips are used for the following:
-Configure and manage Azure virtual machines
-Create and deploy Docker containers
-Deploy Ansible playbooks to manage virtual machines
-Launch ELK Stack
The final setup includes a jumpbox hosting ansible container which connects to three seperate virtual machines.There are two machines (for redundancy) hosting DVWA and one server hosting ELK Stack.The above set up is protected by a firewall configured with a whitelist and secured ports.

* Automated ELK Stack Deployment


The files in this repository were used to configure the network depicted below.

<img src="https://github.com/dharanik28/Project-1/blob/main/diagrams/Topology.PNG">

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.


[ELK Playbook](https://github.com/dharanik28/Project-1/blob/main/ansible/elk-playbook.yml)


This document contains the following details:
* Description of the Topology
* Access Policies
* ELK Configuration
  * Beats in Use
  * Machines Being Monitored
* How to Use the Ansible Build




* Description of the Topology


The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.


Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- The load balancer aids in security by offloading traffic from a corporate server to a public cloud provider. 
Integrating an ELK (Elasticsearch, Logstash, and Kibana) server allows users to easily monitor the vulnerable VMs for changes to the file names and watch system metrics.


Filebeat monitors the logs in the specified locations. It detects changes to the filesystem. It collects logs and forwards them to Elastisearch or Logstash for indexing


Metricbeat detects changes in system metrics such as CPU usage. We use it to detect SSH login attempts


The configuration details of each machine may be found below.

<img src="https://github.com/dharanik28/Project-1/blob/main/diagrams/configurationDetailsofEachMachine.jpg">


* Access Policies


The machines on the internal network are not exposed to the public Internet. 


Only the Jumpbox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the designated public IP address. This address changes each time the machine is powered down and restarted. 


Machines within the network can only be accessed by peer servers. The Jumpbox Provisioner connects via SSH to the Webservers (Web 1, Web 2 & Web 3) and the ELK Server. The Web Server machines send logs to the ELK Server to be forwarded for indexing.


A summary of the access policies in place can be found in the table below.
  
<img src="https://github.com/dharanik28/Project-1/blob/main/diagrams/AccessPolicyconfigImage.jpg">


*  Elk Configuration


Ansible was used to automate configuration of the ELK Server. No configuration was performed manually, which is advantageous because...
* Using Ansible allowed me to quickly install, update, and add the web servers to the network using the same playbooks


The playbook implements the following tasks:
* Install docker on all network machines so they will be able to recieve and install containers
* Ansible is installed on the Jump Box VM to distribute containers to other VMs on the network
* Ansible playbooks are used to install the ELK stack container on the ELK server and a 'Beats' containers on the Web servers

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<img src="https://github.com/dharanik28/Project-1/blob/main/diagrams/docker_ps_output.PNG">

Target Machines & Beats
This ELK server is configured to monitor the following machines:

10.0.0.5
10.0.0.6
10.0.0.7

We have installed the following Beat on these machines:

filebeat-7.6.1-amd64.deb
This Beat allow us to collect the following information from each machine:

Filebeat is used to send your log files to kibana. Filebeat monitors and collects log events on specificed servers.


We have installed the following Beats on these machines:


* Filebeat
* Metricbeat


These Beats allow us to collect the following information from each machine:


* Filebeat - Filebeat detects changes to the filesystem. We are using this to monitor our Web Log data.
* Metricbeat - Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.


The Ansible Playbooks used can be found here

ansible-playbook [elk-playbook.yml](https://github.com/dharanik28/Project-1/blob/main/ansible/elk-playbook.yml)

ansible-playbook [filebeat-playbook.yml](https://github.com/dharanik28/Project-1/blob/main/ansible/filebeat-playbook.yml)

Ansible-playbook [metricbeat-playbook.yml](https://github.com/dharanik28/Project-1/blob/main/ansible/metricbeat-playbook.yml)

* Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 


SSH into the control node and follow the steps below:
- Copy the .yml files to /etc/ansible directory.
- Update the hosts file to be as follows. This will assign the VM servers to their server groups for the Ansible Playbooks.


```
   [webservers]
    10.0.0.5
    10.0.0.6
    10.0.0.7
   [elkservers]
    10.1.0.4
```
	
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

```
cd /etc/ansible
ansible-playbook elk-playbook.yml
ansible-playbook filebeat-playbook.yml
Ansible-playbook metricbeat-playbook.yml
```

Then navigate to curl http://168.61.23.94:5601/app/kibana#/home (Kibana) to check that the installation worked as expected. 

