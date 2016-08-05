# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # Install docker
    sudo apt-get install -y curl wget git vim
    curl -fsSL https://get.docker.com/ | sh
    sudo usermod -aG docker vagrant

    # Enable docker remote api
    sudo cp /vagrant/systemd/docker-tcp.socket /etc/systemd/system/
    sudo systemctl daemon-reload
    sudo systemctl enable docker-tcp.socket
    sudo systemctl stop docker
    sudo systemctl start docker-tcp.socket
    sudo systemctl start docker
  SHELL

  config.vm.define "swarm-manager1" do |c|
    c.vm.hostname = "swarm-manager1.example.com"
    c.vm.network "private_network", ip: "192.168.60.11"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm init --advertise-addr enp0s8
      docker swarm join-token -q manager > /vagrant/tokens/manager
      docker swarm join-token -q worker > /vagrant/tokens/worker
    SHELL
  end

  config.vm.define "swarm-manager2" do |c|
    c.vm.hostname = "swarm-manager2.example.com"
    c.vm.network "private_network", ip: "192.168.60.12"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr enp0s8 --token `cat /vagrant/tokens/manager` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-manager3" do |c|
    c.vm.hostname = "swarm-manager3.example.com"
    c.vm.network "private_network", ip: "192.168.60.13"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr enp0s8 --token `cat /vagrant/tokens/manager` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node1" do |c|
    c.vm.hostname = "swarm-node1.example.com"
    c.vm.network "private_network", ip: "192.168.60.21"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr enp0s8 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node2" do |c|
    c.vm.hostname = "swarm-node2.example.com"
    c.vm.network "private_network", ip: "192.168.60.22"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr enp0s8 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end

  config.vm.define "swarm-node3" do |c|
    c.vm.hostname = "swarm-node3.example.com"
    c.vm.network "private_network", ip: "192.168.60.23"

    c.vm.provision "shell", inline: <<-SHELL
      docker swarm join --advertise-addr enp0s8 --token `cat /vagrant/tokens/worker` 192.168.60.11:2377
    SHELL
  end
end
