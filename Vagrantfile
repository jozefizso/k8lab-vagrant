# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "vmware_desktop" do |vb|
    vb.gui = true
    vb.vmx["memsize"] = 1024
    vb.vmx["numvcpus"] = 2
  end

  config.vm.define "k8s-master" do |master|
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.hostname = "k8s-master"

    master.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yml"
    end
  end

  (1..2).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
      node.vm.hostname = "node-#{i}"

      node.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "kubernetes-setup/node-playbook.yml"
      end
    end
  end
end
