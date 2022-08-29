BOX_IMAGE = "generic/debian11"
NODE_COUNT = 2

Vagrant.configure("2") do |config|

  # master
  config.vm.define "k3s-master" do |node|
    node.vm.box = BOX_IMAGE
    node.vm.hostname = "k3s-master"
    node.vm.network "private_network", ip: "192.168.56.100", hostname: true
    node.vm.provider "virtualbox" do |vb|
      vb.name = "k3s-master"
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end
  end

  # workers
  (1..NODE_COUNT).each do |i|
    config.vm.define "k3s-worker-#{i}" do |node|
      node.vm.box = BOX_IMAGE
      node.vm.hostname = "k3s-worker-#{i}"
      node.vm.network "private_network", ip: "192.168.56.#{100 + i}", hostname: true
      node.vm.provider "virtualbox" do |vb|
        vb.name = "k3s-worker-#{i}"
        vb.memory = "2048"
        vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
      end
    end
  end
end
