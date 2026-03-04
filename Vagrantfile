Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # Admin poste
  config.vm.define "admin" do |admin|
    admin.vm.hostname = "admin"
    admin.vm.network "private_network", ip: "192.168.50.10"
    admin.vm.provider "virtualbox" do |vb|
      vb.name   = "admin"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end

  # Masters (control planes)
  (1..2).each do |i|
    config.vm.define "master-#{i}" do |m|
      m.vm.hostname = "master-#{i}"
      m.vm.network "private_network", ip: "192.168.50.1#{i}"
      m.vm.provider "virtualbox" do |vb|
        vb.name   = "master-#{i}"
        vb.memory = 2048
        vb.cpus   = 2
      end
    end
  end

  # Workers
  (1..2).each do |i|
    config.vm.define "worker-#{i}" do |w|
      w.vm.hostname = "worker-#{i}"
      w.vm.network "private_network", ip: "192.168.50.2#{i}"
      w.vm.provider "virtualbox" do |vb|
        vb.name   = "worker-#{i}"
        vb.memory = 2048
        vb.cpus   = 2
      end
    end
  end

# Load Balancer
  config.vm.define "lb-node" do |lb|
    lb.vm.hostname = "lb-node"
    lb.vm.network "private_network", ip: "192.168.50.100"
    lb.vm.provider "virtualbox" do |vb|
      vb.name   = "lb"
      vb.memory = 1024
      vb.cpus   = 1
    end
  end
end

