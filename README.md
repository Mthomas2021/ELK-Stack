# ELK-Stack
## Automated ELK Stack Deployment Project

The files in this repository were used to configure the network depicted below.


![https://github.com/Mthomas2021/ELK-Stack/blob/main/Diagrams/Network%20Diagram.png] (Images/Network Diagram.png)



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

 https://github.com/Mthomas2021/ELK-Stack/blob/main/Ansible/dvwa.yml 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application. Load balancing ensures that the application will be highly protected, in addition to restricting traffic to the network. Azure Load Balancer operates at layer four of the Open Systems Interconnection (OSI) model and protects servers from getting overloaded and possibly breaking down. 

If a single server goes down, the load balancer redirects traffic to the remaining online servers. When a new server is added to the server group, the load balancer automatically starts to send requests to it.  Jump box prevents Azure VMs from being exposed to the public. This means that this will be the entry point connecting via Remote Desktop Protocol (RDP) from the on-premise network. It also helps to open only one port instead of several ports to connect different virtual machines present in the Azure cloud.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic by using filebeat and metricbeat. Filebeats collects log data and metricbeat collects metrics as their names denote. 


The configuration details of each machine may be found below.



|        Name        	|       Function       	|     IP Address     	|    Operating system    	|
|:------------------:	|:--------------------:	|:------------------:	|:----------------------:	|
|      Jumpbox       	|        Gateway       	|      10.0.0.4      	|          Linux         	|
|        Web1        	|   Webserver - DVWA   	|      10.0.0.5      	|          Linux         	|
|        Web2        	|   Webserver - DVWA   	|      10.0.0.6      	|          Linux         	|
|       Elk-VM       	|    Virtual Machine   	|      10.1.0.4      	|          Linux         	|


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

-Personal workstation public IP 

Machines within the network can only be accessed by SSH from Jumpbox.

The Elk VM is only accessible by SSH from the Jumpbox. Its IP address is 10.0.0.4 (Jumpbox Private IP)

A summary of the access policies in place can be found in the table below.

|        Name        	| Publicly Accessible 	|        Allowed IPs     	    				|
|:--------------------:	|:---------------------:|:---------------------------------------------:|
|      Jumpbox       	|          No         	|      PCs Public IP port-22					|
|        Web1        	|          No         	|        10.0.0.4        	    				|
|        Web2        	|          No         	|        10.0.0.4        	    				|
|       Elk-VM       	|         yes         	|        10.0.0.4 /52.142.62.151port 5601       |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it will be safeguarded from vulnerabilities.

- Free: Ansible is an open-source tool.

- Very simple to set up and use: No special coding skills are necessary to use Ansible's playbooks.

- Powerful: Ansible lets user model even highly complex IT work flows.

- Flexible: User can orchestrate the entire application environment no matter where it’s deployed. Users can also customize it based on their needs.

- Agentless: User don’t need to install any other software or firewall ports on the client systems it want to automate. User also don’t have to set up a separate management         structure.

- Efficient: Because user don’t need to install any extra software, there’s more room for application resources on the server.


The playbook implements the following tasks:

- Install docker.io

- Install pip3

- Install Docker python module

- Increase virtual memory

- Download and launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK stack.

(Images/docker_ps_output.png)

### Target Machines & Beats

The ELK server is configured to monitor the following machines:


|  Machines | IP Address                   |
|-----------|------------------------------|
|  Web1	    |10.0.0.5                      |
|  Web2	    |10.0.0.6                      |



We have installed the following Beats on these machines: 

- Filebeat 

- Metricbeat

Developers describe Filebeat as "A lightweight shipper for forwarding and centralizing log data". It watches and monitors the log files or locations that users specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. 
	eg: syslog events, host-names and processes, visitors by OS

Metricbeat is detailed as "A Lightweight Shipper for Metrics". It collects metrics from the systems and servers. It records and periodically collects metrics from the operating system and from services running on the server and takes the metrics and statistics that it collects and ships them to the output that users specify, such as Elasticsearch or Logstash.
	eg: inbound traffic, disk and memory usage
	
These information can then be viewed through Kibana through customizable charts and tables

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:


- Copy the YAML files (.yml) to Ansible's dirctory.

- In order to use Ansible to configure the machine, you must add it to the list of machines Ansible can discover and connect to.

	  /etc/ansible/hosts
		[webservers]
		10.0.0.5 ansible_python_interpreter=/usr/bin/python3
		10.0.0.6 ansible_python_interpreter=/usr/bin/python3
		
		[elk]
		10.1.0.4 ansible_python_interpreter=/usr/bin/python3

		This list is Ansible's inventory and is stored in the hosts text file:

- Update the playbook file to include - name/apt (name of the playbook), hosts (the group of servers in the hosts file that we will run actions on), become (run as root on the server we are configuring), tasks (specify what actions we want to takes place)

- Run the playbook, and navigate to Elk VM and Kibana to check that the installation worked as expected.

Playbooks are the files where Ansible code is written in YAML format (Yet Another Markup Language). They are like a to-do list for Ansible that contains a list of tasks. It must be copied to the Ansible directory.

Ansible playbook to be updated before we can run it in the Ansible. The inventory file, usually named "hosts" contains the IP addresses of the machines you want to configure with Ansible. IP addresses are usually put in groups. You will use "group" names in playbooks to specify which machines to run the playbook on . Eg: to install Elk the group mentioned as "elkservers", to install filebeat on Web1 and Web2 the group to be mentioned as "webserver"

Navigate to the public IP of the Load Balancer from your workstation to confirm it was successful, and can be accessed from a browser.


#Specific commands used in ELK stack Deployment

	Create a new playbook: 
		touch /etc/ansible/install-elk.yml

	Run the playbook: 
		ansible-playbook ./elk-playbook.yml 

	Check the docker status: 
		sudo service docker status

	Start the docker: 
		sudo service docker start

	To see docker running with correct ports: 
		sudo docker ps -a

	To get filebeat installation file: 
		curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

	To go to kibana: 
		http://<ELK.VM.Public.IP>:5601/app/kibana from a browser
