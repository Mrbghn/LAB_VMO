# install package virtualenv
python3 -m pip install --user virtualenv

#  If it is already present upgrade it:
python3 -m pip install --upgrade virtualenv

# create a new virtual environment Python 3.12
virtualenv test --python=python3.12
source test/bin/activate

# Update something for good things
virtualenv --upgrade-embed-wheels
virtualenv --reset-app-data
python3.12 -m pip install --upgrade setuptools

# Create folder for Ansible
mkdir ~/Desktop/ansible-playbook
cd ~/Desktop/ansible-playbook

# Create an inventory file to store the server
touch inventory.ini && sudo nano inventory.ini
------------------------------------------
[servers]
server1 ansible_host=10.15.0.181 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem
server2 ansible_host=10.15.0.182 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem
server3 ansible_host=10.15.0.183 ansible_user=admin ansible_ssh_private_key_file=/home/chinhnd/TungVMO/admin.pem

[all:vars]
ansible_python_interpreter=/usr/bin/python3
------------------------------------------

# Create playbook to play
touch install_git.yml && sudo nano install_git.yml
------------------------------------------
- name: Install Git
  hosts: servers
  gather_facts: false
  remote_user: admin
  become: true
  tasks:
    - name: Install base GIT
      shell: sudo apt-get update && sudo apt-get install libz-dev libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext cmake gcc curl -y
     
    - name: Create tmp folder
      shell: cd /home/admin && mkdir tmp
        
    - name: Download Git from internet
      shell: cd /home/admin/tmp && curl -o git.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.42.0.tar.gz

    - name: Extract Git tarball
      shell: cd /home/admin/tmp && tar -zxvf git.tar.gz

    - name: Install Git from source
      shell: cd /home/admin/tmp/git-* && make prefix=/usr/local all && sudo make prefix=/usr/local install && exec bash
------------------------------------------

# Let's play
ansible-playbook -i inventory.ini install_git.yml

## Remember check again in VM for the good things
