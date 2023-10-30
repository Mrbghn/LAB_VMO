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

# Install Ansible with pip
python3.12 -m pip install ansible
ansible --version

# Create folder for Ansible
mkdir ~/Desktop/ansible-playbook
cd ~/Desktop/ansible-playbook

# Create an inventory file to store the server
touch inventory.ini && sudo nano inventory.ini

# Create playbook to play
touch install_git.yml && sudo nano install_git.yml

# Let's play
ansible-playbook -i inventory.ini install_git.yml

## Remember check again in VM for the good things
