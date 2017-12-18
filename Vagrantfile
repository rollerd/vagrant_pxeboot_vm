#!/usr/bin/env bash
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Pin this Vagrantfile to specific Vagrant version
Vagrant.require_version ">=2.0.0", "<=2.0.0"

# Begin configuring Vagrant VM
Vagrant.configure("2") do |config|
    vm_name = 'default'
	config.vm.box = "rollerd/centos7py36"

    # Configure network
    config.vm.network "public_network"

    # Configure shared folders
    config.vm.synced_folder "./isos", "/vagrant/isos", id: "vagrant-root", :mount_options => ["dmode=777","fmode=777"]

    # Configure VM resources
	config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", 4096]
		v.customize ["modifyvm", :id, "--cpus", "2"]
		v.customize ["modifyvm", :id, "--ioapic", "on"]
	end
    # Configure VM networking
    config.vm.hostname = "pxeboot.localhost"
    config.ssh.insert_key = false

    # Copy provisioning directory to guest
    config.vm.provision "file", source: "./provisioning", destination: "/tmp/"

    # Attempt to re-sync the time server
    config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/America/New_York /etc/localtime", run: "always"

    # Run ansible provisioner playbook from the ansible.playbook path on the guest
	config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "/tmp/provisioning/provision_vagrant.yml"
        ansible.compatibility_mode = "2.0"
       # ansible.verbose = true
    end
end
