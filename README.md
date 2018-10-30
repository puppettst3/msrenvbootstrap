# Ansible Bootstrap EC2 instances with roles.

## Assumptions:
   1. EC2 hostnames customization based off Route53 domain.
   2. Default security group is going to allow traffic to port 80, 5984 ( we can move this to ansible if required )
   
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
5. Install ansible on the instance.
<br /> sudo apt-get update && sudo apt-get install ansible -y
6. Inside /etc/ansible/hosts define groups and vars required.

   <br />[all:vars]
   <br />ansible_python_interpreter=/usr/bin/python3 
   <br />[docker]
   <br />MSR-test-instance-1
   <br />MSR-test-instance-2
   <br />[webservers:vars]
   <br />container=webserver
   <br />[webservers]
   <br />MSR-test-instance-1
   <br />[couchDB:vars]
   container=couchDB
   <br />[couchDB]
   <br />MSR-test-instance-2
  
7. Create the EC2 instances by running below environment.
   <br />ansible-playbook msrtestenv.yml

8. Due to Public zone access capability lack we are going to hardcode the instance IP's into /etc/hosts
9. Log into accept ssh key by logging into them ( This is one of the future improvement )
10. Next run the ansible dockerization playbook.
   <br />ansible-playbook msritcustom.yml

## Resources:
   1. http://scraplab.net/custom-ec2-hostnames-and-dns-entries/
   2. http://github.com/geerlingguy/ansible-role-docker
   3. https://github.com/geerlingguy/ansible-role-pip/
   4. https://hub.docker.com/_/httpd/
   5. https://hub.docker.com/_/couchdb/
   
## One more way to simplify the installation and custom hostnames
<br /> Servers behind private subnets
<br /> No Public IP
<br /> Autoscaling groups in use.
   https://eazevedoaws.wordpress.com/2016/04/24/ansible-and-aws-provisioning-ec2-instances/
