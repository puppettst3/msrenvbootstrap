# Ansible Bootstrap EC2 instances with roles.

## Assumptions:
   1. EC2 hostnames customization based off Route53 domain.
   
## Improvements for Future:
    1. Need to check if EC2 instance already provisioned if yes do not re-provision
    2. Need to merge the 2 playbooks.
    3. ssh-keyscan mechanism to scan and propagate the keys to inventory of hosts.
    4. Remove hardcoded Route53 zone.
    5. Parameterized role in msrtestenv to use with_items or any loop of group hosts passed like groups.docker in our case.
    6. EC2 security groups as code.

## How to provision and assign the roles.

1. Create a AWS account to use.
2. Create a role with privileges required to provision EC2 instances and administer Route53 domain.
3. Assign the role created to the EC2 ansible instnace so as to not to hardcode any user keys in the code.
4. Create a github account to store the code.
5. Inside /etc/ansible/hosts define groups and vars required.

[all:vars]
ansible_python_interpreter=/usr/bin/python3

[docker]
MSR-test-instance-1
MSR-test-instance-2

[webservers:vars]
container=webserver

[webservers]
MSR-test-instance-1

[couchDB:vars]
container=couchDB

[couchDB]
MSR-test-instance-2

   ansible-playbook msrtestenv.yml
   
   ansible-playbook msritcustom.yml

## Resources:
   1. http://scraplab.net/custom-ec2-hostnames-and-dns-entries/
   2. http://github.com/geerlingguy/ansible-role-docker
   3. https://github.com/geerlingguy/ansible-role-pip/
   4. https://hub.docker.com/_/httpd/
   5. https://hub.docker.com/_/couchdb/
