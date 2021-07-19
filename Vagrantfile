# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:


# Define the number of worker nodes
NUM_WORKER_NODE = 1
IMAGE_NAME = "ubuntu/bionic64"


Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end
  config.disksize.size = "50GB"

  # k8s cluster setup

  # k8s master setup
  config.vm.define "cks-master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "cks-master"
    master.vm.network "private_network", ip: "192.168.50.10"

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yml"
      ansible.extra_vars = { node_ip: "192.168.50.10", }
    end
  end

  # k8s worker setup
  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "cks-worker-#{i}" do |node|

      node.vm.box = IMAGE_NAME
      node.vm.hostname = "cks-worker-#{i}"
      node.vm.network "private_network", ip: "192.168.50.#{i + 10}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/node-playbook.yml"
        ansible.extra_vars = { node_ip: "192.168.50.#{i + 10}",
                               kubelet_file: "cks-worker-#{i}.conf"}
      end
    end
  end

end
