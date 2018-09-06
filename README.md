# ansible-docker-ee-ubuntu

## Overview
ansible playbook to install docker EE on ubuntu 16.04

references: 
https://docs.docker.com/install/linux/docker-ee/ubuntu/#install-docker-ee
https://docs.docker.com/install/linux/linux-postinstall/


## Content
dockerEE_ubuntu.yml: main playbook 
dockerEE_creds.yml: contains dockerEE-related variables 
dockerEE_ubuntu_addon.yml: additional tasks for docker group and systemctl setup


## HOWTO

step 1: edit /etc/ansible/hosts with list of ubuntu nodes and proper python_interpreter variable - sample extract below

```
[ubuntuaws2]
10.1.1.10
10.1.1.11
10.1.1.12

[ubuntuaws2:vars]
ansible_python_interpreter=/usr/bin/python3
```




step 2: rename dockerEE_creds_sample.yml to dockerEE_creds.yml

step 3: update content of dockerEE_creds.yml with appropriate data

step 4: check host reachability / ssh keys 

```
ansible -m ping ubuntuaws2 --user=ubuntu --private-key=mykey.pem
```

step 4: run the playbook 

```
ansible-playbook dockerEE_ubuntu.yml --user=ubuntu --private-key=mykey.pem --become
```


note: "--become" option overlaps with use of task-based become - due to missing become option in apt_key

