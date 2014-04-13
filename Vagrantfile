# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Silly but effective syntax for overriding "[default]" box name
  config.vm.define :nodebox do |t|
  end

  config.vm.provider :virtualbox do |vb, override|
    override.vm.box = "saucy64"
    override.vm.hostname = "nodebox"

    # Canonical builds nightly Vagrant images for Ubuntu: http://cloud-images.ubuntu.com/vagrant/
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :forwarded_port, guest: 80, host: 7777
    config.vm.network :forwarded_port, guest: 7777, host: 7777

    config.vm.network :private_network, ip: "192.168.33.10"

    # vb.customize ["modifyvm", :id, "--memory", "1024"]
    # vb.customize ["modifyvm", :id, "--cpus", "2"]

  end

  config.vm.provider :aws do |aws, override|

    # We need a box to base this image on, so point Vagrant to a dummy one.
    config.vm.box = "dummy-aws"
    config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

    # Make sure these environment variables are populated in the host OS 
    # (i.e. the machine deploying the VM).
    aws.access_key_id = ENV['AWS_ACCESS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET_KEY']
    override.ssh.private_key_path = ENV['EC2_PRIVATE_KEY']
    override.ssh.username = "ubuntu"

    # Set these security options in the EC2 Console
    aws.security_groups = [ 'vagrant' ]
    aws.keypair_name = "vagrant-aws"

    # Specify the instance type here, e.g. [ "t1.micro", "t1.small", "t1.large" ]
    aws.instance_type = "t1.micro"

    # Canonical maintains an index for EC3 images: http://cloud-images.ubuntu.com/locator/ec2/
    # us-east-1 raring 13.04 amd64 instance-store ami-ad83d7c4
    aws.ami = "ami-ad83d7c4"
    aws.region = "us-east-1"

  end

  # config.vm.synced_folder "../data", "/vagrant_data"
  #
  config.vm.provision :ansible do |ansible|

     ansible.playbook = "playbook.xml"
     ansible.inventory_file = "hosts" 
     ansible.verbose = "extra"
  end

end
