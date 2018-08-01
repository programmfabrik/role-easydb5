# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
  end

  config.vm.provision "shell" do |s|
    s.inline = <<-SHELL
      mkdir -p /root/.ssh
      chmod 750 /root/.ssh
    SHELL
  end

  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
  config.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"
  # ... any additional SSH keys can go here

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "node0" do |node|
    node.vm.box = "bento/ubuntu-16.04"
    node.vm.network "private_network", ip: "10.42.0.2"
    node.vm.hostname = "node0.pf.vagrant"
    node.vm.synced_folder ".", "/vagrant"
    node.vm.network "forwarded_port", guest: 80, host: 8080
  end
end