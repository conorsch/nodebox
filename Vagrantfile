# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :virtualbox do |vb, override|
    override.vm.box = "raring64"
    override.vm.hostname = "nodebox"

    # Enable vagrant-cachier support, so packages aren't downloaded for every provisioning.
    config.cache.auto_detect = true
    config.cache.enable :apt
    config.cache.enable :chef

    # Canonical builds nightly Vagrant images for Ubuntu: http://cloud-images.ubuntu.com/vagrant/
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"

    config.vm.network :forwarded_port, guest: 80, host: 7777
    config.vm.network :forwarded_port, guest: 7777, host: 7777

    config.vm.network :private_network, ip: "192.168.33.10"

    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]

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

  config.vm.provision :chef_solo do |chef|

    chef.cookbooks_path = "cookbooks"
    chef.add_recipe "mongodb"
    chef.add_recipe "nodejs::install_from_binary"
    chef.add_recipe "nodejs::npm"

    # At the time of this writing, node v0.10.20 was current. 
    # If you want to use a newer version of node, update the values below.
    # The checksums are here: http://nodejs.org/dist/v0.10.20/SHASUMS256.txt
    chef.json = { 
        :nodejs => { 
          :version => "0.10.20",
          :checksum_linux_x64 => "eaebfc66d031f3b5071b72c84dd74f326a9a3c018e14d5de7d107c4f3a36dc96",
          :checksum_linux_x86 => "4dc94e7de766523f6427b9de75dd3e4f1d3d15d01464e03d98f9c96e09769746",
        }
    #    :npm => { version => "1.3.5" }
    }

  end

  $script = <<SCRIPT
  echo 'Checking package dependencies for subsonic2...'
  locale-gen en_US.UTF-8
  sudo aptitude install -y ffmpeg
  cd /vagrant && npm install
  /usr/bin/mongod  --port 27017 --dbpath /var/lib/mongodb --logpath /var/log/mongodb/mongodb.log --smallfiles &
  cd /vagrant && node .
SCRIPT

  config.vm.provision "shell", inline: $script

end
