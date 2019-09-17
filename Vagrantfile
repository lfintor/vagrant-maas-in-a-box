# -*- mode: ruby -*-
# vi: set ft=ruby :

# Setup our MAAS and node boxes
# MAAS will use Virtualbox and 1024M RAM
# Nodes will use Virtualbox and 512M RAM
# Recommended to have at least 6G RAM with roughly 2G free disk space


Vagrant.configure("2") do |config|
	  
# Define our MAAS instance
	config.vm.define "maas", primary: true do |maas|
		maas.vm.box = "ubuntu/bionic64"
		maas.vm.hostname = "maascontroller"
		maas.vm.network :private_network, ip: '192.168.50.99'
		maas.vm.network :forwarded_port, guest: 80, host: 8080
		maas.vm.provider "virtualbox" do |vbox|
			vbox.gui = true
			vbox.name = "maascontroller"
			vbox.customize ["modifyvm", :id, "--memory", "2048"]
		end
		maas.vm.provision "shell",
			inline: "apt update"
		maas.vm.provision "shell",
			inline: "apt upgrade -y"
		maas.vm.provision "shell",
			inline: "apt autoremove -y"
		maas.vm.provision "shell",
			inline: "apt install language-pack-en-base -y"
		maas.vm.provision "shell",
			inline: "apt install python-apt -y"
		maas.vm.provision "shell",
			inline: "apt install python-pycurl -y"
		maas.vm.provision "shell",
			inline: "apt install ubuntu-cloud-keyring -y"
		maas.vm.provision "shell",
			inline: "apt install maas -y"
		maas.vm.provision "shell",
			inline: "apt install maas-dns -y"
		maas.vm.provision "shell",
			inline: "apt install maas-dhcp -y"
		maas.vm.provision "shell",
			inline: "add-apt-repository ppa:juju/stable -y"
		maas.vm.provision "shell",
			inline: "apt update && sudo apt-get install juju -y"			
	end

# Define node
	config.vm.define "node1" do |node1|
		node1.vm.box = "ubuntu/bionic64"
		node1.vm.hostname = "node1"
		node1.vm.network :private_network, ip: "192.168.50.100"
		node1.vm.provider "virtualbox" do |vbox|
			vbox.gui = false
			vbox.name = "node1"
			vbox.customize ["modifyvm", :id, "--memory", "512"]
			vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
			vbox.customize ["modifyvm", :id, "--boot1", "net"]
			vbox.customize ["modifyvm", :id, "--boot2", "disk"]
		end
		node1.vm.provision "shell",
			inline: "apt update"
		node1.vm.provision "shell",
			inline: "apt upgrade -y"
		node1.vm.provision "shell",
			inline: "apt autoremove -y"
	end
end
