# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Pugins
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = true
  end

  # OS
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "1024"

  end
end
