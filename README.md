# Week13HW
Repository for my homework

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram](Ansible/Project-1.drawio)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

  - [Filebeat playbook](Ansible/filebeatplaybook.yml)
  - [Metricbeat playbook](Ansible/metricbeatplaybook.yml)
  - [Elk playbook](Ansible/elkplaybook.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancers help to ensure the availability through transportation of data going to servers. Jump boxes are for easier control over multiple servers and systems.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.
- Filebeat is used to make logs on specific directories or files.
- Metricbeat is used to collect metrics from the system and services which helps you to monitor the servers better.

The configuration details of each machine may be found below.

| Name    | Function   | Ip Address | Operating System |
|---------|------------|------------|------------------|
| Jumpbox | Gateway    | 10.0.0.4   | Linux (Ubuntu)   |
| Web 1   | Web Server | 10.0.0.5   | Linux (Ubuntu)   |
| Web 2   | Web Server | 10.0.0.6   | Linux (Ubuntu)   |
| ELK     | Log Server | 10.1.0.4   | Linux (Ubuntu)   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 10.0.0.4, 10.0.0.5

Machines within the network can only be accessed by the personal computer.
- Only machine have access to ELK is my own IP through 5601.

A summary of the access policies in place can be found in the table below.

| Name    | Publicly Accessible | Allowed Ip Address |
|---------|---------------------|--------------------|
| Jumpbox | Yes                 | My own IP          |
| Web 1   | No                  | Jumpbox IP         |
| Web 2   | No                  | Jumpbox IP         |
| ELK     | Yes                 | My own IP          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because system installtion and update can be more simole and effective and able to limit the services. Services and proccesses can be also replicated easily.
- The advantage of automating configuration with Ansible is to able to make many services at once which is good for companies that are in need for many of them.

The playbook implements the following tasks (Don't know why they are all in one line, it's not like this in the text editor):
- Installs docker.io, python3-pip and the Docker module
   `name: Install docker.io
    apt:
     update_cache: yes
     force_apt_get: yes
     name: docker.io
     state: present`

   `name: Install python3-pip
    apt:
     force_apt_get: yes
     name: python3-pip
     state: present`
   
   `name: Install Docker module
    pip:
     name: docker
     state: present`
- Increases the memory through command for virtual machine
   `name: Increase virtual memory
    command: sysctl -w vm.max_map_count=262144`
- Uses more memory with sysctl
   `name: Use more memory
    sysctl:
      name: vm.max_map_count
      value: 262144
      status: present
      reload: yes`

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/Bradenkh1/Week13HW/blob/main/Ansible/2021-09-20%2022_32_15-azadmin%40ELK.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5 (Web 1)
- 10.0.0.6 (Web 2) 

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects logs from local files on the machine. It monitors on the dicectories or/and files then fowards it to Logstash.
- Metricbeat collects metrics and information on the system. It is for checking on the system health.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to Web server VMs.
- Update the hosts file to include those Web servers and ELK server 
- Run the playbook, and navigate to http://20.51.226.102:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it? Files with .yml and it should be copied to ansible folder.
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? Use the config playbook and update the host files to specify which machines.
- Which URL do you navigate to in order to check that the ELK server is running?http://20.51.226.102:5601/app/kibana 
