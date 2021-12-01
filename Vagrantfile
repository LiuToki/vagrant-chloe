# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Plugins
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = true
  end

  # OS
  config.vm.box = "generic/rocky8"

  # Network
  config.vm.network "forwarded_port", guest:3000, host:3000

  # Allura
  # config.vm.network "forwarded_port", guest:8080, host:8080
  # config.vm.network "forwarded_port", guest:8983, host:8983
  # config.vm.network "forwarded_port", guest:8825, host:8825
  # config.vm.network "forwarded_port", guest:27017, host:27017

  # virtual box settings.
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "1024"

  end

  # Provisions

  # dev tools
  $setup_dev_tools = <<-SHELL
    dnf update
    dnf install -y epel-release
    dnf install -y git make wget tar curl zip unzip cmake
  SHELL

  # dev tools のインストール
  config.vm.provision "shell", inline:$setup_dev_tools

  # gcc
  $setup_gcc = <<-SHELL
    dnf install -y gcc-c++
    dnf install -y glibc-devel gmp-devel mpfr-devel libmpc-devel

    wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-11.2.0/gcc-11.2.0.tar.gz
    tar xzvf gcc-11.2.0.tar.gz
    cd gcc-11.2.0
    ./contrib/download_prerequisites

    mkdir build
    cd build
    ../configure --prefix=/usr  --program-suffix=-11.2.0 --enable-languages=c,c++ --disable-multilib
    export LD_LIBRARY_PATH=/usr/local/gcc-11.2.0/lib:$LD_LIBRARY_PATH
    export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu
    export C_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
    export CPLUS_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
    make -j 8
    make install

    cd ../../
    rm -f gcc-11.2.0.tar.gz
    rm -rf gcc-11.2.0

    dnf remove -y gcc gcc-c++
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11.2.0 1 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-11.2.0
  SHELL

  # gcc のインストール
  # config.vm.provision "shell", inline:$setup_gcc

  # docker-ce & docker-compose
  $setup_docker = <<-SHELL
    dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    dnf install -y docker-ce docker-ce-cli
    systemctl enable docker
    systemctl start docker
    curl -L https://github.com/docker/compose/releases/download/v2.1.1/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    SHELL

  # docker-ce & docker-compose のインストール
  config.vm.provision "shell", inline:$setup_docker

  $setup_vagrant = <<-SHELL
    usermod -aG docker vagrant
    # git clone https://gitbox.apache.org/repos/asf/allura.git/ allura-git
  SHELL

  # vagrant 関係
  # config.vm.provision "shell", inline:$setup_vagrant

end
