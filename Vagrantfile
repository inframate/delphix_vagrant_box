# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'time'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# box sizing
VB_MEMORY=1024
VB_CPUS=1

# generic setup
DOMAIN_NAME="delphix.local"
SETUP_HOME='/home/delphix'

IP_BASE = '172.16.138'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Every Vagrant environment requires a box to build off of.
  config.vm.box = "bento/centos-7.1"

  # network config to get this box going
  config.vm.hostname = "box.#{DOMAIN_NAME}"
  config.vm.network "public_network", ip: "#{IP_BASE}.100"

  # size of the box
  config.vm.provider :virtualbox do |vb|
    vb.memory = VB_MEMORY
    vb.cpus = VB_CPUS
  end

  # disable typical plug-ins
  if Vagrant.has_plugin?("vagrant-hostmanager") then
    # manage /etc/hosts on both host & guests
    config.hostmanager.enabled = false # manage guest /etc/hosts
    config.hostmanager.manage_host = false # manage host's /etc/hosts
  end

  # update guest additions
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  # add the delphix user
  config.vm.provision "shell", path: "setup/prepare_delphix.sh"

  # common provisioning tasks, used on all environments
  config.vm.provision "shell", path: "setup/prepare_system.sh"

  # add databases etc
  config.vm.provision "shell", path: "setup/install_mysql.sh"
  config.vm.provision "shell", path: "setup/install_postgres.sh"

  # add runtimes
  config.vm.provision "shell", path: "setup/install_openjdk.sh"
  config.vm.provision "shell", path: "setup/install_ruby.sh"

  # finalize the delphix user setup
  config.vm.provision "shell", path: "setup/setup_delphix.sh"

end
