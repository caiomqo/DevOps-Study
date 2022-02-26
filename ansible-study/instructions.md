# Ansible Documentation
    https://docs.ansible.com/ansible/latest/
    
# Installing Ansible in CentOS 
yum update -y
yum install epel-release -y
yum install ansible -y

# If building lab in Linode 
- Build one VM to be the "master" (will have ansible installed)
- Build other VMs to be "controlled"

# Configuration
- Go to /etc/ansible/
- Edit "hosts" to have the desired IP Addresses
        [linux]

        192.53.160.188
        45.79.52.139

        [linux:vars]

        ansible_user=root
        ansible_password=caiocaio18

- Edit ansible.cfg: Uncomment "host_key_checking"

# Commands
    ### uses module ping to reach devices in linux group
    ansible linux -m ping 

    ### use ad-hoc commands from linux
    ansible linux -a "cat /etc/os-release"
    ansible linux -a "reboot"

# Playbooks
- YAML files with several configs to be applied to devices

- create yaml file with the playbook (example is iluvnano.yaml)
- run in ansible:
  ansible-playbook file.yaml

- ansible will only make a change if the change is needed

