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

if num_nodes_str = ENV['MAAS_NUM_NODES']
  num_nodes = num_nodes_str.to_i + 1
else
  num_nodes = 10
end

if ENV['MAAS_NODE_GUI']
  gui_mode = true
  puts "Running in gui mode"
else
  gui_mode = false
end

# IP's range at 192.168.50.[101:1xx] - gives you 99 nodes plus 1 maas instance
# Keep this in mind when configuring your cluster controller's min/max ip ranges
BOXES = { maas: { ip: "192.168.50.100" } }
(1..num_nodes).each do |i|
  if i < 10
    gen_ip = "192.168.50.10#{i}"
  else
    gen_ip = "192.168.50.1#{i}"
  end
  BOXES["node#{i}".to_sym] = { ip: gen_ip }
end

Vagrant.configure("2") do |config|
  # Define our MAAS instance
  config.vm.define "maas", primary: true do |maas|
    maas.vm.box = "maas"
    maas.vm.hostname = "maascontroller"
    maas.vm.network :private_network, ip: BOXES[:maas][:ip]
    maas.vm.network :forwarded_port, guest: 80, host: 8080
    maas.vm.provider "virtualbox" do |vbox|
      vbox.gui = gui_mode
      vbox.name = "maascontroller"
      vbox.customize ["modifyvm", :id, "--memory", "#{MAAS_MEM}", "--nicpromisc3", "allow-all"]
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
        vbox.gui = gui_mode
        vbox.name = "#{node_name}"
        vbox.vm.box_url = "http://files.vagrantup.com/precise64.box"
        vbox.customize ["modifyvm", :id, "--memory", "#{NODE_MEM}"]
        vbox.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
        vbox.customize ["modifyvm", :id, "--boot1", "net"]
        vbox.customize ["modifyvm", :id, "--boot2", "disk"]
      end
      vm_conf.vm.provision "ansible" do |ansible|
        ansible.playbook = "deploy/nodes.yml"
      end
    end
  end
end
