# Using Vagrant and Ansible with AWS
# Notice: Requires a Linux Debian Netinst VM
# Configure Linux

apt-get update

apt-get upgrade

apt-get install sudo

adduser ronaldo

usermod -aG sudo ronaldo

su ronaldo

# Install VirtualBox

sudo apt-get install -y software-properties-common

sudo apt-add-repository 'deb http://download.virtualbox.org/virtualbox/debian stretch contrib'

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -

wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -

sudo apt-get update

sudo apt-get install virtualbox-5.2

# Install Vagrant

Download install package from https://www.vagrantup.com/downloads.html

sudo dpkg -i vagrant_2.2.3_x86_64.deb

sudo apt-get install rsync

vagrant plugin install vagrant-aws

vagrant plugin install fog-aws

vagrant plugin install --plugin-version 1.0.1 fog-ovirt

vagrant plugin expunge --force --reinstall

vagrant plugin install vagrant-aws

vagrant box add --force dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

# Install Ansible

sudo apt-get install python ansible

# Download and Edit Vagrant & Ansible script files from here

https://github.com/rwing2k/vagrant-ansible-aws

# Bring up machines

vagrant up --provider=aws

# Provision software on machines

ansible-playbook -i hosts provisioning.yml

# Connect ssh

vagrant status

vagrant ssh machine_name

# Destroy Vagrant´s AWS machines
vagrant destroy -f
