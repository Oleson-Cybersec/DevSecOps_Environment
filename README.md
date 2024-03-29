## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/Oleson-Cybersec/DevSecOps_Environment/blob/main/Pictures/Red_Team_Topology.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook.yml files may be used to install only certain pieces of it, such as Filebeat or Metricbeat.


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the web applications have high avalibilty, in addition to evenly distributing incoming traffic to the network evenly.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logging data and system integrity.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 52.183.72.48     | Linux |
| Web--1   | DVWA     | 52.246.248.118   | Linux |
| Web--2   | DVWA     | 52.246.248.118   | Linux |
| Web--3   | DVWA     | 52.246.248.118   | Linux |
| Elk_VM   | Elk      | 40.122.64.99     | Linux |
| LB | Loadbalancer   | 52.246.248.118  | Windows |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox provisoner virutal machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

    -My public IP address

Machines within the network can only be accessed by the internal Web VM's.

| Jumpbox | Web--1 | Web--2 | Web--3 |
|---------|--------|--------|--------|
| 10.1.0.4 | 10.1.0.5 | 10.1.0.6 | 10.1.0.7 |

A summary of the access policies in place can be found in the table below.

| Name    | Publicly Accessible | Allowed IP Addresses |
|---------------------|-------|------------------------------|
| Jump Box          | Yes     | Personal IP Address only     |
| ELK VM            | No      | Personal IP Address only     |
| Web--1            | No      | 10.1.0.4, 10.1.0.6, 10.1.0.7 |
| Web--2            | No      | 10.1.0.4, 10.1.0.5, 10.1.0.7 |
| Web--3            | No      | 10.1.0.4, 10.1.0.5, 10.1.0.6 | 
| Loadbalancer      | Yes     | Any IP Address               |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous for duplication and scalablity purposes. This also ensures that everything is properly installed as writing the codes will either complete, or throw error messages at the user until they're properly fixed. Once fixed, they can run the configuration files and you can sit back as Docker updates the virtual machines specified in files.

The playbook implements the following tasks:
* Install and enable docker.io
* Install python3-pip & python container
* Download ELK stack image
* Change minimum to 262144 to run ELK
* Open the three ports for Elasticsearch, Logstash, Kibana

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/OlesonCrypto/DevSecOps_Environment/blob/main/Pictures/Docker_PS_Output.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 52.246.248.118 (Public IP address of the loadbalancer that distributes to the Web VM's)

We have installed the following Beats on these machines:
* Filebeats
* Metricbeats

These Beats allow us to collect the following information from each machine:
* Filebeats logs files and activities of a system to review on Kibana
* Metricbbeats periodically collects metrics and statistics of a system to review on Kibana

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the docker-playbook.yml file to your jumpbox.
- Update the file to include the appropriate IP addresses.
- Run the playbook, and navigate to http://40.122.64.99:5601/app/kibana#/home/tutorial/systemLogs to check that the filebeat installation worked as expected.
- To verify metricbeat, navigate to http://40.122.64.99:5601/app/kibana#/home/tutorial/dockerMetrics

To navigate to verify the ELK server is running, check your ELK's Public IP
  * http://40.122.64.99:5601/app/kibana#/discover?_g=(refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))&_a=(columns:!(_source),index:'filebeat-*',interval:auto,query:(language:kuery,query:''),sort:!(!('@timestamp',desc)))
  
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
To run the playbooks, and update the files, use the following: 
    - ansible-playbook {install_file}.yml {hostgroup}
