ansible playbook to install docker EE on ubuntu 16.04

dockerEE_ubuntu.yml : main playbook
dockerEE_creds.yml : contains dockerEE-related variables


#HOWTO

step 1: edit /etc/ansible/hosts with list of ubuntu nodes

step 2: run the playbook 
ansible-playbook dockerEE_ubuntu.yml --user=ubuntu --private-key=mykey.pem --become

note: "--become" option overlaps with use of task-based become - due to missing become option in apt_key

