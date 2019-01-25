# -*- mode: ruby -*-
# vi: set ft=ruby :

# NOTE: The following variables must be set according your aws configuration:
$AWS_ACCESS_KEY='XXXXXXXXXXXXXXXXXXXXXXX'                              
$AWS_SECRET_KEY='XXXXXXXXXXXXXXXXXXXXXXX'
$AWS_PRIVATE_KEY_PATH='key-devops-vagrant.pem'
$AWS_DEFAULT_REGION='us-east-1'
#$AWS_DEFAULT_AZ='us-east-1a'
$AWS_EC2_KEY_NAME='key-devops-vagrant'
$AWS_SECURITY_GROUP_NAME='devops-vagrant'

class Host
	def initialize(name, env, ami, user, type, disk)
		@vm_name = name
		@env_name = env
		@amazon_image = ami
		@shh_username = user
		@instance_type = type
		@disk_size_gb = disk
	end	
	def getVMname
		@vm_name
	end
	def getEnviromentname
		@env_name
	end
	def getImagename
		@amazon_image
	end
	def getSSHuser
		@shh_username
	end
	def getInstancetype
		@instance_type
	end
	def getDisksize
		@disk_size_gb
	end
end

# AMI Data
# tag: machine name, tag: enviroment, amazon machine image, SSH username, machine size, GB disk size
HOSTS = []
HOSTS << Host.new('webserver', 'vagrant-test', 'ami-9a562df2', 'ubuntu', 't2.micro', 10)
HOSTS << Host.new('dbserver', 'vagrant-test', 'ami-9a562df2', 'ubuntu', 't2.micro', 10)

HOSTS.each do | machine |
  Vagrant.configure(2) do |config|

    config.vm.box = 'aws'
	config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    config.vm.synced_folder '.', '/vagrant', disabled: true

	config.vm.define machine.getVMname do |vm_name|
      vm_name.vm.provider :aws do |aws, override|
        aws.access_key_id        = $AWS_ACCESS_KEY
        aws.secret_access_key    = $AWS_SECRET_KEY
        aws.region               = $AWS_DEFAULT_REGION
        #aws.availability_zone   = $AWS_DEFAULT_AZ
        aws.keypair_name         = $AWS_EC2_KEY_NAME
        aws.security_groups      = $AWS_SECURITY_GROUP_NAME
        aws.ami                  = machine.getImagename
        aws.instance_type        = machine.getInstancetype
        aws.block_device_mapping = [{
          'DeviceName'     => '/dev/sda1',
          'Ebs.VolumeSize' => machine.getDisksize
          }]
        aws.tags = {
          'Name'        => machine.getVMname,
          'Environment' => machine.getEnviromentname
          }
        # Credentials to login to EC2 Instance
        override.ssh.username         = machine.getSSHuser
        override.ssh.private_key_path = $AWS_PRIVATE_KEY_PATH
      end

      #Configuration for Ansible as Provisioner
      # vm_name.vm.provision :ansible do |ansible|
       # ansible.playbook          = 'playbook.yml'
       # #verbose options: false, v, vvvv
	   # ansible.verbose           = false
       # ansible.host_key_checking = false
       # ansible.limit             = 'all'
      # end
	  
	  #
	  # ansible-playbook playbook.yml -i hosts
	  #
    end
  end
end