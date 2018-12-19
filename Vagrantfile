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

  # Provisions
  config.vm.provision "docker" do |d|
    
  end

  # docker-composeのインストール
  $get_compose = <<-'EOF'
  curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
  chmod +x /usr/local/bin/docker-compose
  EOF
  config.vm.provision "shell", inline: $get_compose 
  
end
