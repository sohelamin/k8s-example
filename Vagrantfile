# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX = "hashicorp/bionic64"

Vagrant.configure("2") do |config|
  config.vm.define "master" do |subconfig|
    subconfig.vm.box = BOX
    subconfig.vm.hostname = "master"
    subconfig.vm.network :private_network, ip: "192.168.50.101"

    subconfig.vm.provider "virtualbox" do |vb|
      vb.cpus = 2
    end
  end

  config.vm.define "worker1" do |subconfig|
    subconfig.vm.box = BOX
    subconfig.vm.hostname = "worker1"
    subconfig.vm.network :private_network, ip: "192.168.50.102"
  end

  config.vm.define "worker2" do |subconfig|
    subconfig.vm.box = BOX
    subconfig.vm.hostname = "worker2"
    subconfig.vm.network :private_network, ip: "192.168.50.103"
  end

  # Provision
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install docker.io -y
    systemctl start docker
    systemctl enable docker
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
    apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
    apt-get install kubeadm kubelet kubectl -y
    apt-mark hold kubeadm kubelet kubectl
    swapoff -a
  SHELL

end