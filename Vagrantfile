# coding: utf-8
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", ip: "192.168.99.99"
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.provision "file", source: "local.conf", destination: "/tmp/local.conf"
  config.vm.provision "shell", inline: <<-SHELL
    sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu%http://ftp.jaist.ac.jp/pub/Linux/ubuntu%g" /etc/apt/sources.list
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get -y upgrade
    useradd -s /bin/bash -d /opt/stack -m stack
    echo "stack ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/stack

    cd /opt/stack
    su stack -c 'git clone -b stable/rocky --depth 1 https://git.openstack.org/openstack-dev/devstack'
    cd /opt/stack/devstack
    su stack -c 'cp /tmp/local.conf .'
    su stack ./stack.sh
  SHELL
end
