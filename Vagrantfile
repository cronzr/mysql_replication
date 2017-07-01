# vagrant


VAGRANTFILE_API_VERSION = "2"
BASE_IMAGE = "debian/jessie64"


$script = <<SCRIPT

apt-get update -qq && apt-get install -y vim curl mysql-server haproxy haproxyctl

SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # ansible
  provision_config = {
      :playbook => "setup.yml",
      :inventory_path => "hosts",
      :verbose => "v"
  }
                   

  config.vm.define "mysql0" do |box|
    box.vm.box = BASE_IMAGE
 
    box.vm.hostname = "mysql0"

    box.vm.provider :lxc do |lxc|
      lxc.customize 'cgroup.memory.limit_in_bytes', '256M'
      lxc.customize 'network.type', 'veth'
      lxc.customize 'network.flags', 'up'
      lxc.customize 'network.name', 'eth0'
      lxc.customize 'network.veth.pair', 'veth-01'
      lxc.customize 'network.link', 'br0'
      lxc.customize 'network.ipv4', '192.168.88.200/24'
     end
  end

  config.vm.define "mysql1" do |box|
    box.vm.box = BASE_IMAGE

    box.vm.hostname = "mysql1"

    box.vm.provider :lxc do |lxc|
      lxc.customize 'cgroup.memory.limit_in_bytes', '256M'
      lxc.customize 'network.type', 'veth'
      lxc.customize 'network.flags', 'up'
      lxc.customize 'network.name', 'eth0'
      lxc.customize 'network.veth.pair', 'veth-02'
      lxc.customize 'network.link', 'br0'
      lxc.customize 'network.ipv4', '192.168.88.201/24'
     end
  end

  config.vm.provision "shell", inline: $script

end
