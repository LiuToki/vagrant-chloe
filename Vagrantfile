# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Plugins
  # if Vagrant.has_plugin?("vagrant-vbguest")
  #   config.vbguest.auto_update = true
  # end

  # virtual box settings.
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "1024"

  end

  # OS
  config.vm.box = "centos/7"

  # public folder
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  # Network
  config.vm.network "forwarded_port", guest:3000, host:3000

  # Allura
  # config.vm.network "forwarded_port", guest:8080, host:8080
  # config.vm.network "forwarded_port", guest:8983, host:8983
  # config.vm.network "forwarded_port", guest:8825, host:8825
  # config.vm.network "forwarded_port", guest:27017, host:27017

  # Provisions

  # dev tools
  $setup_dev_tools = <<-SHELL
    yum update
    yum install -y epel-release
    yum install -y git make wget tar curl zip unzip cmake
  SHELL

  # dev tools のインストール
  config.vm.provision "shell", inline:$setup_dev_tools

  $setup_gcc = <<-SHELL
    yum install -y gcc-c++
    yum install -y glibc-devel gmp-devel mpfr-devel libmpc-devel

    wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-9.2.0/gcc-9.2.0.tar.gz
    tar xzvf gcc-9.2.0.tar.gz
    cd gcc-9.2.0
    ./contrib/download_prerequisites

    mkdir build
    cd build
    ../configure --prefix=/usr  --program-suffix=-9.2.0 --enable-languages=c,c++ --disable-multilib
    export LD_LIBRARY_PATH=/usr/local/gcc-9.2.0/lib:$LD_LIBRARY_PATH
    export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu
    export C_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
    export CPLUS_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
    make -j 4
    make install

    cd ../../
    rm -f gcc-9.2.0.tar.gz
    rm -rf gcc-9.2.0

    yum remove -y gcc gcc-c++
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9.2.0 1 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-9.2.0
  SHELL

  # gcc のインストール
  config.vm.provision "shell", inline:$setup_gcc

  $setup_docker = <<-SHELL
    yum install -y yum-utils
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum-config-manager --disable docker-ce-edge
    yum makecache fast
    yum install -y docker-ce
    systemctl enable docker
    systemctl start docker
    curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    SHELL

  # docker-ce & docker-compose のインストール
  config.vm.provision "shell", inline:$setup_docker

  $setup_vagrant = <<-SHELL
    # adduser vagrant
    usermod -aG docker vagrant
    # git clone https://gitbox.apache.org/repos/asf/allura.git/ allura-git
  SHELL

  # vagrant 関係
  config.vm.provision "shell", inline:$setup_vagrant

end
