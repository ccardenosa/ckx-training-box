# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:


# Define the number of worker nodes
NUM_MASTER_NODES = 1
NUM_WORKER_NODES = 1
IMAGE_NAME = "ubuntu/bionic64"

IP_HEAD="192.168.56."

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end
  config.vm.disk :disk, name: "k8s-vol", size: "50GB"

  (1..NUM_MASTER_NODES).each do |i|
    config.vm.define "master-#{i - 1}" do |node|

      node.vm.box = IMAGE_NAME
      node.vm.hostname = "master-#{i - 1}"
      node.vm.network "forwarded_port", guest: 22, host: "220#{i}"
      node.vm.network "private_network", ip: "#{IP_HEAD}1#{i}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/playbook.yml"
        ansible.verbose = true
        ansible.inventory_path = "inventories/vagrant.inventory"
        ansible.skip_tags = "post-settings"
        ansible.extra_vars = {
          vagrant: true,
          leader_master_ip: "#{IP_HEAD}11",
          node_ip: "#{IP_HEAD}1#{i}",
        }
      end
    end
  end

  (1..NUM_WORKER_NODES).each do |i|
    config.vm.define "worker-#{i - 1}" do |node|

      node.vm.box = IMAGE_NAME
      node.vm.hostname = "worker-#{i - 1}"
      node.vm.network "forwarded_port", guest: 22, host: "220#{i + NUM_MASTER_NODES}"
      node.vm.network "private_network", ip: "#{IP_HEAD}1#{i + NUM_MASTER_NODES}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/playbook.yml"
        ansible.verbose = true
        ansible.inventory_path = "inventories/vagrant.inventory"
        ansible.skip_tags = "post-settings"
        ansible.extra_vars = {
          vagrant: true,
          leader_master_ip: "#{IP_HEAD}11",
          node_ip: "#{IP_HEAD}1#{i + NUM_MASTER_NODES}",
        }
      end
    end
  end

  config.vm.define "worker-#{NUM_WORKER_NODES - 1}" do |node|

    node.vm.provision "ansible" do |ansible|
      ansible.playbook = "kubernetes-setup/playbook.yml"
      ansible.verbose = true
      ansible.inventory_path = "inventories/vagrant.inventory"
      ansible.tags = "post-settings"
      ansible.limit="all"
      ansible.extra_vars = {
        vagrant: true,
        leader_master_ip: "#{IP_HEAD}11",
      }
    end
  end
end
