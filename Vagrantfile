# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box = "fedora/31-cloud-base"
  config.ssh.insert_key = false

  config.vm.define "master" do |master|
    master.vm.provider "virtualbox" do |vb|
      vb.memory = 2500
      vb.cpus = 3
    end
    master.vm.host_name = "master"
    master.vm.network "private_network", ip: "172.17.195.20"
    master.vm.network "forwarded_port", guest: 9090, host:9090

    sharedDir = '/home/vagrant/shared'
    master.vm.synced_folder '.', sharedDir #, disabled: true
    master.vm.provision "shell", inline: <<-SHELL
      dnf -y install python3-pip
      su vagrant -c 'pip3 install ansible --user'
    SHELL
    # vagrant plugin install vagrant-guest_ansible
    # https://github.com/vovimayhem/vagrant-guest_ansible
    provisioner = Vagrant::Util::Platform.windows? ? :guest_ansible : :ansible
    master.vm.provision provisioner do |ansible|
      ansible.inventory_path = "ansible_inventory"
      ansible.playbook = "playbooks/ansible-cockpit.yaml"
    end
  end
end
