# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "raring64"

  # Canonical builds nightly Vagrant images for Ubuntu: http://cloud-images.ubuntu.com/vagrant/
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 80, host: 7777
  config.vm.network :forwarded_port, guest: 7777, host: 7777

  config.vm.network :private_network, ip: "192.168.33.10"

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

  config.vm.provision :chef_solo do |chef|

    chef.cookbooks_path = "cookbooks"
    chef.add_recipe = "apt"
    chef.add_recipe "nodejs::install_from_binary"
    chef.add_recipe "nodejs::npm"
    chef.json = { 
        :nodejs => { 
        :version => "0.10.20",
      }
    }

  end

  $script = <<SCRIPT
  echo Checking package dependencies for subsonic2...
  sudo aptitude install -y ffmpeg mongodb
  cd /vagrant && npm install
  node .
SCRIPT

  config.vm.provision "shell", inline: $script

end
