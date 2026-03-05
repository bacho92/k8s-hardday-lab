# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_version = "20241008.0.0"   # stable 22.04

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus   = 2
    vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
  end

  # Liste complète des /etc/hosts (identique sur toutes les VMs)
  hosts_content = <<-EOF
192.168.50.10  admin.kubernetes.local admin jumpbox
192.168.50.100 lb.kubernetes.local lb lb-node
192.168.50.11  master-1.kubernetes.local master-1
192.168.50.12  master-2.kubernetes.local master-2
192.168.50.21  worker-1.kubernetes.local worker-1
192.168.50.22  worker-2.kubernetes.local worker-2
  EOF

  # ====================== ADMIN / JUMPBOX ======================
  config.vm.define "admin" do |node|
    node.vm.hostname = "admin.kubernetes.local"
    node.vm.network "private_network", ip: "192.168.50.10"
    node.vm.provision "shell", inline: <<-SHELL
      apt-get update -qq
      apt-get install -y -qq curl git wget vim net-tools
      echo '#{hosts_content}' >> /etc/hosts
    SHELL
  end

  # ====================== LOAD BALANCER ======================
  config.vm.define "lb" do |node|
    node.vm.hostname = "lb.kubernetes.local"
    node.vm.network "private_network", ip: "192.168.50.100"
    node.vm.provision "shell", inline: <<-SHELL
      apt-get update -qq
      apt-get install -y -qq haproxy
      echo '#{hosts_content}' >> /etc/hosts
    SHELL
  end

  # ====================== MASTERS ======================
  (1..2).each do |i|
    config.vm.define "master-#{i}" do |node|
      node.vm.hostname = "master-#{i}.kubernetes.local"
      node.vm.network "private_network", ip: "192.168.50.1#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = 4096   # recommandé pour etcd + apiserver + controller-manager
      end
      node.vm.provision "shell", inline: "echo '#{hosts_content}' >> /etc/hosts"
    end
  end

  # ====================== WORKERS ======================
  (1..2).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.hostname = "worker-#{i}.kubernetes.local"
      node.vm.network "private_network", ip: "192.168.50.2#{i}"
      node.vm.provision "shell", inline: "echo '#{hosts_content}' >> /etc/hosts"
    end
  end
end
