# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key", "~/.ssh/id_rsa"]
  config.ssh.insert_key = false
  config.vm.box = "debian/buster64"


  ## MASTERS
  config.vm.define :proxy do |proxy|
    proxy.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.75"
      proxy.vm.hostname = 'proxy'
      proxy.vm.provider :libvirt do |domain|
        domain.memory = 4096
        domain.cpus = 4
        domain.machine_virtual_size = 100
     end
   end
  config.vm.define :master1 do |master1|
    master1.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.76"
    master1.vm.hostname = 'master1'
      master1.vm.provider :libvirt do |domain|
        domain.memory = 8192
        domain.cpus = 4
        domain.machine_virtual_size = 100
     end
   end
  config.vm.define :master2 do |master2|
    master2.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.77"
    master2.vm.hostname = 'master2'
      master2.vm.provider :libvirt do |domain|
        domain.memory = 8192
        domain.cpus = 2
        domain.machine_virtual_size = 100
     end
   end
 
  ## NODES

  config.vm.define :node1 do |node1|
    node1.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.78"
    node1.vm.hostname = 'node1'
      node1.vm.provider :libvirt do |domain|
        domain.memory = 4096
        domain.cpus = 2
        domain.machine_virtual_size = 100
     end
  end
 config.vm.define :node2 do |node2|
    node2.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.79"
    node2.vm.hostname = 'node2'
      node2.vm.provider :libvirt do |domain|
        domain.memory = 4096
        domain.cpus = 2
        domain.machine_virtual_size = 100
     end
  end
  config.vm.define :node3 do |node3|
    node3.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.80"
    node3.vm.hostname = 'node3'
      node3.vm.provider :libvirt do |domain|
        domain.memory = 4096
        domain.cpus = 2
        domain.machine_virtual_size = 100
     end
  end
  config.vm.define :node4 do |node4|
    node4.vm.network "public_network",
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      :ip => "192.168.1.81"
    node4.vm.hostname = 'node4'
      node4.vm.provider :libvirt do |domain|
        domain.memory = 4096
        domain.cpus = 2
        domain.machine_virtual_size = 100
     end
  end
end
