# coding: utf-8
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network "private_network", ip: "192.168.99.99"
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.provision "file", source: "local.conf", destination: "/tmp/local.conf"
  config.vm.provision "shell", inline: <<-SHELL
    yum update
    yum install -y git
    useradd -s /bin/bash -d /opt/stack -m stack
    echo "stack ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/stack

    sudo -u stack sh -c 'cd /opt/stack && git clone -b stable/rocky --depth 1 https://git.openstack.org/openstack-dev/devstack'
    sudo -u stack sh -c 'cp /tmp/local.conf /opt/stack/devstack'
    sudo -u stack sh -c 'cd /opt/stack/devstack && ./stack.sh'
  SHELL
end
