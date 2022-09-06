BOX_IMAGE = "generic/debian11"
WORKERS = 2

Vagrant.configure("2") do |config|

  # worker nodes
  (1..WORKERS).each do |worker_no|
    config.vm.define "k3s-worker-#{worker_no}" do |node|
      node.vm.box = BOX_IMAGE
      node.vm.hostname = "k3s-worker-#{worker_no}"
      node.vm.network :private_network, ip: "192.168.56.#{100 + worker_no}", hostname: true
      node.vm.provider :virtualbox do |vb|
        vb.name = "k3s-worker-#{worker_no}"
        vb.memory = "2048"
        vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
      end
    end
  end

  # master node
  config.vm.define "k3s-master" do |node|
    node.vm.box = BOX_IMAGE
    node.vm.hostname = "k3s-master"
    node.vm.network :private_network, ip: "192.168.56.100", hostname: true
    node.vm.provider :virtualbox do |vb|
      vb.name = "k3s-master"
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
    end

    # provisioning
    node.vm.provision :ansible do |ansible|
      ansible.limit = "all"
      ansible.playbook = "site.yml"
    end
  end
end
