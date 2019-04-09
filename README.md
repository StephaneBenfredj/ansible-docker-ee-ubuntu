# ansible-docker-ee-ubuntu

## Overview
ansible playbook to install docker EE on ubuntu 16.04

References:
- https://docs.docker.com/install/linux/docker-ee/ubuntu/#install-docker-ee
- https://docs.docker.com/install/linux/linux-postinstall/


## Content
- dockerEE_ubuntu.yml: main playbook

- dockerEE_creds.yml: contains dockerEE-related variables 

- dockerEE_ubuntu_addon.yml: additional tasks for docker group and systemctl setup


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

Alternatively, create hosts file in local directory and point to it using -i hosts option when using playbooks



step 2: rename dockerEE_creds_sample.yml to dockerEE_creds.yml

step 3: update content of dockerEE_creds.yml with appropriate data

step 4: check host reachability / ssh keys 

```
ansible -m ping ubuntuaws2 --user=ubuntu --private-key=mykey.pem
```

step 4: run the playbooks 

```
ansible-playbook dockerEE_ubuntu.yml --user=ubuntu --private-key=mykey.pem --become
ansible-playbook dockerEE_ubuntu_addon.yml --user=ubuntu --private-key=mykey.pem --become
```


note: in first playbook "--become" option overlaps with use of task-based become - due to missing option in apt_key


Then, the next step is to install UCP on master node(s):

https://docs.docker.com/ee/end-to-end-install/#step-2-install-universal-control-plane


```
docker container run --rm -it --name ucp \
  -v /var/run/docker.sock:/var/run/docker.sock \
  docker/ucp:3.1.5 install \
  --host-address <node-ip-address> \
  --interactive
```


To install DTR on UCP nodes
```
docker container run -it --rm \
  docker/dtr:2.6.4 install \
  --ucp-node <node-hostname> \
  --ucp-insecure-tls
```