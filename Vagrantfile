
vm_mem = "512"

$host_script = <<SCRIPT
#!/bin/bash
cat > /etc/hosts <<EOF
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.211.55.100 vm-cluster-node1.localdomain vm-cluster-node1
10.211.55.101 vm-cluster-node2.localdomain vm-cluster-node2
EOF
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.define :master do |master|
    master.vm.box = "centos64"
    master.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = vm_mem
    end
    master.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node1"
      v.customize ["modifyvm", :id, "--memory", vm_mem]
    end
    master.vm.hostname = "vm-cluster-node1"
    master.vm.network :private_network, ip: "10.211.55.100"
  end

  config.vm.define :slave1 do |slave1|
    slave1.vm.box = "centos64"
    slave1.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = vm_mem
    end
    slave1.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node2"
      v.customize ["modifyvm", :id, "--memory", vm_mem]
    end
    slave1.vm.hostname = "vm-cluster-node2"
    slave1.vm.network :private_network, ip: "10.211.55.101"
  end

  config.vm.provision :shell, :inline => $host_script
  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = 'puppet/manifests'
    puppet.manifest_file = 'site.pp'
    puppet.module_path = 'puppet/modules'
  end
end