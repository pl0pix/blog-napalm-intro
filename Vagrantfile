# -*- mode: ruby -*-
# vi: set ft=ruby et:

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = "debian/stretch64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false
  if Vagrant.has_plugin?("vagrant-vbguest")
	config.vbguest.auto_update = false
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.define "host1" do |host1|
	host1.vm.box = "debian/stretch64"
	host1.vm.hostname = "napalm"
	# host1.vm.network "forwarded_port", guest: 22, host: 11021
	host1.vm.network "private_network", ip: "192.168.56.101",
					   virtualbox__intnet: 'linkname1',
					   auto_config: true
	host1.vm.network "private_network", ip: "192.168.57.101",
					   virtualbox__intnet: 'linkname2',
					   auto_config: true
	host1.vm.provider :virtualbox do |v|
	  v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	  v.customize ["modifyvm", :id, "--name", "host1"]
	end
	host1.vm.provision "shell", inline: <<-EOF
	  apt-get update
	  # python-2 installation
	  # apt-get install -y lldpd libssl-dev libffi-dev python-dev python-cffi python-pip
	  # su -c "pip install napalm" vagrant
	  # python3 installation
	  sudo apt install -y lldpd python3-pip
	  su -c "pip3 install napalm" vagrant
	EOF
  end
  config.vm.define "switch1" do |switch1|
	# switch1.vm.box = "vEOS-lab-4.20.3F"
	switch1.vm.box = "veos-lab-4.21.1"
	# switch1.vm.hostname = "switch1"
	# switch1.vm.network "forwarded_port", guest: 22, host: 11022
	# switch1.vm.network "forwarded_port", guest: 443, host: 11443
	switch1.vm.network "private_network",
					   ip: "169.254.1.11",
					   virtualbox__intnet: 'linkname1',
					   auto_config: false
	switch1.vm.network "private_network",
					   ip: "169.254.1.11",
					   virtualbox__intnet: 'linkname2',
					   auto_config: false
	# switch1.vm.provider "virtualbox" do |v|
	#	v.gui = true
	#	v.memory = '2048'
	#  v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	#  v.customize ["modifyvm", :id, "--name", "switch1"]
	# end
	switch1.vm.provision "shell", inline: <<-SHELL
	  FastCli -p 15 -c "configure
	 hostname switch1
	 interface Ethernet1
	  no switchport
	  ip address 192.168.56.102/24"
	SHELL
  end
end
