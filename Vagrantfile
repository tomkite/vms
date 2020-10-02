# -*- modeL ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
hosthome = "/Users/tom"
guesthome = "/home/vagrant"
tmpdir = "/tmp/tmpdir"
sshkey = "tom_20200501"
vmdir = "/Users/tom/data/geek/vm"

vm1_name = "vm1"
vm2_name = "vm2"
vm3_name = "vm3"
gateway_ip = "192.168.1.1"
vm1_ip = "192.168.1.221"
vm2_ip = "192.168.1.222"
vm3_ip = "192.168.1.223"
vm1_ip2 = "10.10.1.1"
vm2_ip2 = "10.10.1.2"
vm3_ip2 = "10.10.1.3"
# [tom@tlc ~]$ VBoxManage list bridgedifs
# Name:            en0: Wi-Fi (Wireless)
bridge_if = "en0: Wi-Fi (Wireless)"

sh_cmd = <<eos
   echo "SCRIPT BEGIN..."
   hostname
   w
   date
   echo "SCRIPT END!"
eos

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "generic/ubuntu1804"
  config.ssh.insert_key = true
  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 1
    v.linked_clone = true
  end

  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = vm1_name
    vm1.vm.network "public_network", bridge: bridge_if, ip: vm1_ip , bootproto: "static", gateway: gateway_ip
    vm1.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}.pub", destination: "#{tmpdir}/#{sshkey}.pub"
    vm1.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}", destination: "#{tmpdir}/#{sshkey}"
    vm1.vm.provision "file", source: "sshconfig", destination: "#{tmpdir}/config"
    vm1.vm.provision "shell", privileged: true, inline: "cat #{tmpdir}/#{sshkey}.pub >> #{guesthome}/.ssh/authorized_keys"
    vm1.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/#{sshkey} #{guesthome}/.ssh/"
    vm1.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/config #{guesthome}/.ssh/"
    vm1.vm.provision "shell", privileged: true, inline: "rm -rf #{tmpdir}"
    vm1.vm.synced_folder vmdir, "/home/vagrant/vm"
    vm1.vm.provision "shell", privileged: true, inline: sh_cmd
    vm1.vm.provision "shell", inline: "echo vm1"
    vm1.vm.provider "virtualbox" do |vb|
#      vb.gui = true
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = vm2_name
    vm2.vm.network "public_network", bridge: bridge_if, ip: vm2_ip , bootproto: "static", gateway: gateway_ip
    vm2.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}.pub", destination: "#{tmpdir}/#{sshkey}.pub"
    vm2.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}", destination: "#{tmpdir}/#{sshkey}"
    vm2.vm.provision "file", source: "sshconfig", destination: "#{tmpdir}/config"
    vm2.vm.provision "shell", privileged: true, inline: "cat #{tmpdir}/#{sshkey}.pub >> #{guesthome}/.ssh/authorized_keys"
    vm2.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/#{sshkey} #{guesthome}/.ssh/"
    vm2.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/config #{guesthome}/.ssh/"
    vm2.vm.provision "shell", privileged: true, inline: "rm -rf #{tmpdir}"
    vm2.vm.synced_folder vmdir, "/home/vagrant/vm"
    vm2.vm.provision "shell", inline: "echo vm2"
  end

  config.vm.define "vm3" do |vm3|
    vm3.vm.hostname = vm3_name
    vm3.vm.network "public_network", bridge: bridge_if, ip: vm3_ip , bootproto: "static", gateway: gateway_ip
    vm3.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}.pub", destination: "#{tmpdir}/#{sshkey}.pub"
    vm3.vm.provision "file", source: "#{hosthome}/.ssh/#{sshkey}", destination: "#{tmpdir}/#{sshkey}"
    vm3.vm.provision "file", source: "sshconfig", destination: "#{tmpdir}/config"
    vm3.vm.provision "shell", privileged: true, inline: "cat #{tmpdir}/#{sshkey}.pub >> #{guesthome}/.ssh/authorized_keys"
    vm3.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/#{sshkey} #{guesthome}/.ssh/"
    vm3.vm.provision "shell", privileged: true, inline: "mv #{tmpdir}/config #{guesthome}/.ssh/"
    vm3.vm.provision "shell", privileged: true, inline: "rm -rf #{tmpdir}"
    vm3.vm.synced_folder vmdir, "/home/vagrant/vm"
    vm3.vm.provision "shell", inline: "echo vm3"
  end

end
