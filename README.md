# Ansible Bootstrap EC2 instances with roles.

## Assumptions:
   1. We are going to use Route53 to access the servers using their public hostsname.
   
## Improvements for Future:
    1. Need to check if EC2 instance already provisioned if yes do not re-provision
    2. Need to merge the 2 playbooks.
    3. include ssh-keyscan mechanism to scan and propagate the keys to inventory of hosts.
    4. Remove hardcoded Route53 zone.
    5. Parameterized role in msrtestenv to use with_items or any loop of group hosts passed like groups.docker in our case.
    
## Ansible Roles example taken from
geerlingguy
    
## Environment setup:
In our environment we have provisioned ec2 instances using Ubuntu Images which have python3 installed
Due to this we have to use an ansible variable ansible_python_interpreter for inventory hosts to support the execution.

Inside pip role we have changed default var pip_package to pytho3-pip

## How to provision and assign the roles.

1. Create a AWS account to use.
2. Create a user with EC2 instance to use provision environemnt
3. Create an IAM role with the privileges required to create EC2 instances and Route53 administrative privileges.
4. Assign the role create to the EC2 instnace created so as to not hardcode any user keys in the code.

ansible-playbook msrtestenv.yml
ansible-playbook msritcustom.yml
