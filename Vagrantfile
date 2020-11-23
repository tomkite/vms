# -*- modeL ruby -*-
# vi: set ft=ruby :

hosthome = "/Users/tom.cai"
guesthome = "/home/vagrant"
sshkey = "tomkite_rsa"
vmdir = "#{hosthome}/vms/ubuntu"
sharedir = "#{hosthome}/vms/share"
persistfile = "~/vms/vmdata.vdi"
vm1_name = "vm1"
vm2_name = "vm2"
vm3_name = "vm3"
vm1_ip = "192.168.1.21"
vm2_ip = "192.168.1.22"
vm3_ip = "192.168.1.23"
# [tom@tlc ~]$ VBoxManage list bridgedifs
# Name:            en0: Wi-Fi (Wireless)
bridge_if = "en0: Wi-Fi (Wireless)"
gateway_ip = "192.168.1.1"

# ssh vagrant to start ssh session. check #{vmdir}/files/sshconfig vagrant entry if there are ssh issues

sh_cmd = <<eos
  date
  ls /vmdata 2>/dev/null 1>dev/null && chmod -R 777 /vmdata
  cat #{guesthome}/.ssh/#{sshkey}.pub >> #{guesthome}/.ssh/authorized_keys
  ip -4 a show | grep 192.168 | sed -e"s/^  *//" | cut -d" " -f2 | tee #{guesthome}/ip
eos

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"
  #config.ssh.insert_key = true
  config.vm.synced_folder sharedir, "/home/vagrant/share"
  config.vm.provision "file", source: "#{vmdir}/files/.tmux.conf",    destination: "#{guesthome}/.tmux.conf"
  config.vm.provision "file", source: "#{vmdir}/files/.bash_profile", destination: "#{guesthome}/.bash_profile"
  config.vm.provision "file", source: "#{vmdir}/files/#{sshkey}.pub", destination: "#{guesthome}/.ssh/#{sshkey}.pub"
  config.vm.provision "file", source: "#{vmdir}/files/#{sshkey}",     destination: "#{guesthome}/.ssh/#{sshkey}"
  config.vm.provision "file", source: "#{vmdir}/files/sshconfig",     destination: "#{guesthome}/.ssh/config"
  config.vm.provision "shell", privileged: true, inline: sh_cmd
  config.vm.provider "virtualbox" do |vb|
    #vb.gui = true
    vb.linked_clone = true
    vb.memory = 1024
    vb.cpus = 1
  end

  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = vm1_name
    vm1.vm.network "public_network", bridge: 'en0: Wi-Fi (Wireless)', ip: vm1_ip
    # https://github.com/kusnier/vagrant-persistent-storage
    vm1.persistent_storage.enabled = true
    vm1.persistent_storage.location = "#{persistfile}"
    vm1.persistent_storage.size = 2000
    vm1.persistent_storage.mountname = 'logs'
    vm1.persistent_storage.filesystem = 'ext4'
    vm1.persistent_storage.mountpoint = '/vmdata'
    #config.persistent_storage.volgroupname = 'tomkite' # use default 'vagrant' if not set
    vm1.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    vm1.vm.provision "shell", inline: "echo vm1"
  end

  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = vm2_name
    vm2.vm.network "public_network", bridge: 'en0: Wi-Fi (Wireless)', ip: vm2_ip
    vm2.vm.provision "shell", inline: "echo vm2"
  end

end

