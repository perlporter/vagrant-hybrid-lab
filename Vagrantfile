# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "debian"   => {"memory" => "1024", "cpu" => "1", "ip" => "10", "image" => "debian/buster64"},
  "ubuntu"   => {"memory" => "1024", "cpu" => "1", "ip" => "20", "image" => "ubuntu/bionic64"},
  "centos"   => {"memory" => "1024", "cpu" => "1", "ip" => "30", "image" => "centos/7"},
  "windows"  => {"memory" => "2048", "cpu" => "2", "ip" => "40", "image" => "inclusivedesign/windows10-eval"},
}

Vagrant.configure("2") do |config|

  config.vm.box_check_update = false
  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      if "#{name}" != "windows"
        machine.vm.hostname = "#{name}.example"
      end
      machine.vm.network "private_network", ip: "10.11.11.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/hybrid-lab"]
      end
      if "#{name}" == "windows"
        machine.vm.provision "shell", 
          inline: <<-SHELL
            netsh.exe int ip show addresses
            netsh.exe int ip set address "Ethernet 2" static 10.11.11.#{conf["ip"]} 255.255.255.0 10.11.11.1
        SHELL
      end
    end
  end
end
