# -*- mode: ruby -*-
# vi: set ft=ruby :

# Setup our MAAS and node boxes
# MAAS will use Virtualbox and 1024M RAM
# Nodes will use Virtualbox and 512M RAM
# Recommended to have at least 6G RAM with roughly 2G free disk space

# MAAS Properties
MAAS_MEM = '1024'

# Node Properties
NODE_MEM = '512'

# IP's range at 10.0.3.[100:110]
# Keep this in mind when configuring your cluster controller's min/max ip ranges
# Hardcoded mac addresses are used for nodes 1-10, use them when commissioning nodes
# in the webui.
BOXES = {
  node1: {
    ip: '10.0.3.101',
    mac: '00163e71d5e7',
  },
  node2: {
    ip: '10.0.3.102',
    mac: '00163e6c125e',
  },
  node3: {
    ip: '10.0.3.103',
    mac: '00163e57190c',
  },
  node4: {
    ip: '10.0.3.104',
    mac: '00163e32b203',
  },
  node5: {
    ip: '10.0.3.105',
    mac: '00163e249ef7',
  },
  node6: {
    ip: '10.0.3.106',
    mac: '00163e214337',
  },
  node7: {
    ip: '10.0.3.107',
    mac: '00163e257a65',
  },
  node8: {
    ip: '10.0.3.108',
    mac: '00163e538c23',
  },
  node9: {
    ip: '10.0.3.109',
    mac: '00163e30eaff',
  },
  node10: {
    ip: '10.0.3.110',
    mac: '00163e11b240',
  },
}

Vagrant.configure("2") do |config|
  # Define our MAAS instance
  config.vm.define "maas", primary: true do |maas|
    maas.vm.box = "maas"
    maas.vm.hostname = "maascontroller"
    maas.vm.network :private_network, ip: "10.0.3.100"
    maas.vm.network :forwarded_port, guest: 80, host: 8080
    maas.vm.provider "virtualbox" do |vbox|
      vbox.gui = false
      vbox.name = "maascontroller"
      vbox.customize ["modifyvm", :id, "--memory", "#{MAAS_MEM}"]
      vbox.vm.box_url = "http://files.vagrantup.com/precise64.box"
    end
    maas.vm.provision "ansible" do |ansible|
      ansible.playbook = "deploy/maas.yml"
    end
  end

  # Define our nodes
  BOXES.each do |node_name, node_config|
    config.vm.define(node_name.to_sym) do |vm_conf|
      vm_conf.vm.box = "#{node_name}"
      vm_conf.vm.hostname = "#{node_name}"
      vm_conf.vm.network :private_network, ip: node_config[:ip]
      vm_conf.vm.provider "virtualbox" do |vbox|
        vbox.gui = false
        vbox.name = "#{node_name}"
        vbox.vm.box_url = "http://files.vagrantup.com/precise64.box"
        vbox.customize ["modifyvm", :id, "--memory", "#{NODE_MEM}"]
      end
      vm_conf.vm.provision "ansible" do |ansible|
        ansible.playbook = "deploy/nodes.yml"
      end
    end
  end
end
