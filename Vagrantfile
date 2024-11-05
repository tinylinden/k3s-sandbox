BOX_IMAGE = "generic/debian12"
WORKERS_COUNT = 2
NODE_MEMORY = "4096"
NODE_CPUS = "4"

Vagrant.configure("2") do |config|

  (0..WORKERS_COUNT).each do |node_no|
    config.vm.define "k3s-node-#{node_no}" do |node|
      node.vm.box = BOX_IMAGE
      node.vm.hostname = "k3s-node-#{node_no}"
      node.vm.network :private_network, ip: "192.168.56.#{100 + node_no}", hostname: true
      node.vm.provider :virtualbox do |vb|
        vb.name = "k3s-node-#{node_no}"
        vb.memory = NODE_MEMORY
        vb.cpus= NODE_CPUS
        vb.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
      end

      if node_no == WORKERS_COUNT
        node.vm.provision :ansible do |ansible|
          ansible.limit = "all"
          ansible.playbook = "site.yml"
        end
      end
    end
  end
end
