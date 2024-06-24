# Cloud Security

## Description
This repository contains my Cloud Security project which features the design of a virtual network in Azure and configuration of an automated ELK stack. This project automates the deployment of ELK (Elasticsearch, Logstash, Kibana) stack, simplifying the setup and configuration processes ensuring a scalable, resilient logging and monitoring infrastructure. 

## Features
- Automated ELK stack deployment
- Configurable for different environments
- Scalable and resilient architecture
- Analyze and visualize data for application and infrastructure monitoring, faster troubleshooting, and security analytics
- Comprehensive logging and monitoring

## Automated ELK Stack Architecture
The files in this repository were used to configure the network depicted below.
![Diagram](https://github.com/aele1401/Cloud_Security/blob/main/Images/ELK_NET_Diagram.PNG)

## Getting Started
Prerequisites:
- Linux
- Ansible
- Filebeat
- Metricbeat
- Docker
- Python

### Installation 
1.  Clone repository: `git clone https://github.com/aele1401/Cloud_Security.git`
2.  Navigate to project directory: `cd c:/users/[your username]/Cloud_Security`
3.  Install Linux updates and upgrades: `sudo apt-get update` and `sudo apt-get upgrade`

## Usage & Deployment
### Deployment Instructions - Automated ELK Stack Deployment
An ELK Stack is a collection of open-source tools that is used for data and log analysis, collection, and visualization. 
ELK is comprised of:
* Elasticsearch, a distributed search and analytics engine that supports many languages.
* Logstash, an open-source data ingesting and processing tool that collects data from various resources, processes the data, and transmits the data to a defined destination.
* Kibana, a data visualization and exploration tool used for analytics, monitoring, and use cases.

ELK can be used with SIEM systems to collect system logs and metrics aggregating the data into an integrated environment which can be used to analyze and visualize the data for application and infrastructure monitoring, faster troubleshooting, and security analytics. It can support threat detection and engineering to help develop custom detections tailored to the security environment of a business that helps create a “less detection, more effective” SOC environment while reducing the amounts of false positives. These tools combined can help protect a business and help build a secure environment for business operations.

The files in this repo have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above or portions of the deployment. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
* Description of the Topology
* Access Policies
* ELK Configuration
	- Beats in Use
	- Machines Being Monitored
* How to Use the Ansible Build


## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, a Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to optimizing resources and response.

Integrating an ELK server allows administrators to easily monitor the vulnerable virtual machines for changes to the log files and system metrics.

The configuration details of each machine may be found below.

| Name     | Function     | IP Address     | Operating System |
|----------|--------------|----------------|------------------|
| Jump Box |Gateway       | 10.0.0.7       | Linux            |
| Web 1    |App Server    | 10.1.0.11      | Linux            |
| Web 2    |App Server    | 10.1.0.9       | Linux            |
| Web 3    |App Server    | 10.1.0.12      | Linux            |
| ELK      |ELK Server    | 192.168.1.100  | Linux            |

## Access Policies

The machines on the internal network are not exposed to the public internet. Only the Jump Box and ELK machines can accept connections from the internet. Access to these machine are only allowed from specific IP addresses that are whitelisted. Machines within the internal network can only be accessed by web servers.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Local machine IP     |
| ELK      | Yes                 | Local machine IP     |
| Web VMs  | No                  | 192.168.1.100        |

## ELK Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it automates manual tasks.

The playbook implements the following tasks:
- Installs docker images (docker.io), python module (python3-pip), and enables docker service
- Increases system memory with sysctl module
- Downloads and launches docker containers (web & elk)
- Installs, sets up, and enables filebeat & metricbeat

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker Diagram](https://github.com/aele1401/Cloud_Security/blob/main/Images/dockerps.PNG)

## Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.1.0.11
- Web 2: 10.1.0.09
- Web 3: 10.1.0.12

The following Beats have been installed on these machines:
- Filebeat
- Metricbeat

These Beats allow the administrator to collect the following information from each machine:
* Filebeat to collect file logs that helps with detecting malicious activity and anomalies.
* Metricbeat collects metric information, which shows system health, performance, and resource usage.

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration (preconfigured) files to your ansible playbook.
	* Copy `web.yml` and `elk.yml` files into your `"/etc/ansible/roles/"` directory under your ansible container.
- Attach your ansible docker container.
	* Attach with: `"sudo docker attach [your.container.name]"`
- Update the ansible configuration (ansible.cfg) file to include your ELK server IP address.
	* Navigate to `"/etc/ansible/hosts"` and update the hosts file to include your ELK server IP address.
	* Note: Run `"ansible_python_interpreter=/usr/bin/python3"` to make sure that the correct python module is installed. 
- Run the playbook, and navigate to your kibana site to check that the installation worked as expected.
	* Navigate to `"http://[your.elk.ip]:5601/app/kibana".`

## Using Filebeat
To use filebeat:
- Edit `"/etc/ansible/files/filebeat-config.yml"` in the ansible container on the control node to include the ELK IP address.
	* Note: To ensure security, remember to change the default login credentials.
- Run the playbook file
	* `ansible-playbook /etc/ansible/roles/filebeat-playbook.yml`

## Using Metricbeat

To use Metricbeat:
- Edit `"/etc/ansible/files/metricbeat-config.yml"` in the ansible on the control node to include the ELK IP address.
	* Note: Remember to update the default login credentials to ensure security.
- Run the playbook file
	* `ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml`
