# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_GROUPS = {
              "master" => ["node1"],
              "nodes" => ["node2", "node3", "node4"],
              "all_groups:children" => ["master", "nodes"]
            }


Vagrant.configure(2) do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.synced_folder ".", "/vagrant"

    # Configure the master
    config.vm.define "node1" do |node1|
        node1.vm.network "private_network", ip: "192.168.33.10"
        node1.vm.hostname = "node1"
        node1.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook.yml"
            ansible.groups = ANSIBLE_GROUPS
        end
        config.vm.provider :virtualbox do |vb|
            vb.memory = 2048
            vb.cpus = 2
        end
    end

    # Configure a bunch of slaves
    slaves = {
      "node2" => "192.168.33.11",
      "node3" => "192.168.33.12",
      "node4" => "192.168.33.13",
    }

    slaves.each do |hostname, ip|
        config.vm.define hostname do |node|
            node.vm.network "private_network", ip: ip
            node.vm.hostname = hostname
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "playbook.yml"
                ansible.groups = ANSIBLE_GROUPS
            end
            config.vm.provider :virtualbox do |vb|
              vb.memory = 2048
              vb.cpus = 2
            end
        end
    end
end
