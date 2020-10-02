# Create Vagrant VMs for testing playground

### How to create a ubuntu 18.04 VM playground

* you may need to update Vagrant file before running the following commands
* things to be watched are: ip addresses, network interface name, etc.
  - ubuntu version
  - network configs: bridged public ips
  - ssh keys and host configs

        > $) cd <repo_home>
        > $) vagrant up
        > $) ssh vm1
        > $) vagrant ssh vm2

### Navigate to various VMs

* The playground has three ubuntu18.04 vms, namely vm1, vm2, and vm3 created 
* You can simply ssh <vm_name> from the host machine or the vm machines

