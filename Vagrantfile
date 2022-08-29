Vagrant.configure("2") do |config|

  # k3s-master
  config.vm.define "k3s-master" do |node|
    node.vm.box = "debian/bullseye64"
    node.vm.hostname = "k3s-master"
    node.vm.network "private_network", ip: "192.168.56.100", hostname: true
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k3s-master"
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end
  end

  # k3s-worker-01
  config.vm.define "k3s-worker-01" do |node|
    node.vm.box = "debian/bullseye64"
    node.vm.hostname = "k3s-worker-01"
    node.vm.network "private_network", ip: "192.168.56.101", hostname: true
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k3s-worker-01"
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end
  end

  # k3s-worker-02
  config.vm.define "k3s-worker-02" do |node|
    node.vm.box = "debian/bullseye64"
    node.vm.hostname = "k3s-worker-02"
    node.vm.network "private_network", ip: "192.168.56.102", hostname: true
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k3s-worker-02"
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end
  end
end
