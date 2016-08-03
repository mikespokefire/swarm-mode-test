# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update && sudo apt-get upgrade -y
    sudo apt-get install -y wget git vim
    curl -fsSL https://get.docker.com/ | sh
    sudo usermod -aG docker vagrant
  SHELL

  config.vm.define "swarm-manager1" do |c|
    c.vm.hostname = "swarm-manager1.example.com"
    c.vm.network "private_network", ip: "192.168.60.11"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm init --advertise-addr eth1
      docker swarm join-token -q manager > /vagrant/tokens/manager
      docker swarm join-token -q worker > /vagrant/tokens/worker
    SHELL
  end

  config.vm.define "swarm-manager2" do |c|
    c.vm.hostname = "swarm-manager2.example.com"
    c.vm.network "private_network", ip: "192.168.60.12"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr eth1 --token `cat /vagrant/tokens/manager` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-manager3" do |c|
    c.vm.hostname = "swarm-manager3.example.com"
    c.vm.network "private_network", ip: "192.168.60.13"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr eth1 --token `cat /vagrant/tokens/manager` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node1" do |c|
    c.vm.hostname = "swarm-node1.example.com"
    c.vm.network "private_network", ip: "192.168.60.21"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr eth1 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node2" do |c|
    c.vm.hostname = "swarm-node2.example.com"
    c.vm.network "private_network", ip: "192.168.60.22"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr eth1 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node3" do |c|
    c.vm.hostname = "swarm-node3.example.com"
    c.vm.network "private_network", ip: "192.168.60.23"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr eth1 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end
end
