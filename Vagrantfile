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
    curl -fsSL https://test.docker.com/ | sh
    sudo usermod -aG docker vagrant
  SHELL

  config.vm.define "swarm-manager" do |c|
    c.vm.hostname = "swarm-manager.example.com"
    c.vm.network "private_network", ip: "192.168.60.10"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm init --secret mylittlesecret
    SHELL
  end

  config.vm.define "swarm-node1" do |c|
    c.vm.hostname = "swarm-node1.example.com"
    c.vm.network "private_network", ip: "192.168.60.11"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join 192.168.60.10 --secret mylittlesecret
    SHELL
  end

  config.vm.define "swarm-node2" do |c|
    c.vm.hostname = "swarm-node2.example.com"
    c.vm.network "private_network", ip: "192.168.60.12"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join 192.168.60.10 --secret mylittlesecret
    SHELL
  end

  config.vm.define "swarm-node3" do |c|
    c.vm.hostname = "swarm-node3.example.com"
    c.vm.network "private_network", ip: "192.168.60.13"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join 192.168.60.10 --secret mylittlesecret
    SHELL
  end
end
