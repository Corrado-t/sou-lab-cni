# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'getoptlong'

# Define command-line options
opts = GetoptLong.new(
  [ '--fe-node-number', GetoptLong::OPTIONAL_ARGUMENT ],
  [ '--be-node-number', GetoptLong::OPTIONAL_ARGUMENT ]
)

# Default node number
feNodeNumber = 1
beNodeNumber = 1

opts.ordering = GetoptLong::REQUIRE_ORDER   

# Parse the command-line arguments --
opts.each do |opt, arg|
  case opt
    when '--fe-node-number'
      feNodeNumber = arg.to_i
    when '--be-node-number'
      beNodeNumber = arg.to_i
  end
end

# Vagrant configuration
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  # Create multiple frontEnd VMs based on nodeNumber
  (1..feNodeNumber).each do |i|
    config.vm.define "soufe#{i}" do |node|
      node.vm.network "private_network", ip: "192.168.56.#{i + 1}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      node.vm.provision "shell", inline: "echo hello from front end node #{i}"
    end
  end

   # Create multiple backEnd VMs based on nodeNumber
  (1..beNodeNumber).each do |i|
    config.vm.define "soube#{i}" do |node|
      node.vm.network "private_network", ip: "192.168.56.#{i + 65}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      node.vm.provision "shell", inline: "echo hello from backend node #{i}"
    end
  end
end
