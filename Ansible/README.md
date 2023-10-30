# Install
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt-get install ansible -y

# Setting up Host VM
sudo nano /etc/ansible/hosts
------------------------------
[servers]
server1 ansible_host=10.15.0.181 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem
server2 ansible_host=10.15.0.182 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem
server3 ansible_host=10.15.0.183 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem

[all:vars]
ansible_python_interpreter=/usr/bin/python3
------------------------------

# Check host
ansible-inventory --list -y
ansible all -m ping -u admin
ansible all -a "df -h" -u admin

# Create file *.yml to write command
------------------------------------
- name: Update Ubuntu servers
  hosts: servers
  become: true
  tasks:
    - name: Run apt-get update
      shell: sudo apt-get update
------------------------------------

# Run command
ansible-playbook update.yml
ansible-playbook -i inventory.ini update.yml
(*) inventory.ini same content like /etc/ansible/hosts

# Copy file from VM to Local Machine
ansible server1 -i <path_to_inventory_file .ini> -b -m fetch -a "src=<path/on/VM> dest=<path/on/local>"

# Copy file from Local Machine to VM
ansible server1 -i <path_to_inventory_file .ini> -b -m copy -a "src=<path/on/local> dest=<path/on/VM>"

# Run command only each VM
ansible server1 -b -m shell -a "apt-get update"

# Run playbook for specific server
ansible-playbook --limit=server1 update.yml

# About edit file with command like "nano","vim" on server using Ansible
That NOT recommand, but can use some thing:
## Add new line to file
-----------------------------------------
- name: Edit file on server1
  hosts: server1
  become: true
  tasks:
    - name: Edit file
      lineinfile:
        path: /path/to/file.txt
        line: "New line to add or modify"
-----------------------------------------
## Modify an existing line in a file
-----------------------------------------
- name: Edit file on server1
  hosts: server1
  become: true
  tasks:
    - name: Edit file
      replace:
        path: /path/to/file.txt
        regexp: '^Old line to modify$'
        line: "New line to replace the old line"
-----------------------------------------
