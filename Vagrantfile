# -*- mode: ruby -*-
# vi: set ft=ruby :

# load configs
require 'yaml'
current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yml")
vagrant_config = configs['configs']

# require necessary plugins
# for sshfs on windows, follow:
# https://docs.microsoft.com/de-de/windows-server/administration/openssh/openssh_install_firstuse
required_plugins = %w( vagrant-vbguest vagrant-hostmanager )
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end
# required_plugins = %w( vagrant-vbguest vagrant-sshfs )
# required_plugins.each do |plugin|
#   system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
# end

Vagrant.configure("2") do |config|

  # activate hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  #config.vm.box = "ubuntu/focal64"
  config.vm.box = "ubuntu/bionic64"

  # set memory to 2048m
  config.vm.provider "virtualbox" do |vb|
    vb.memory = vagrant_config['memory']
    vb.cpus = vagrant_config['cpus']
  end

  # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
  config.vm.network "private_network", ip: vagrant_config['ip']
  config.vm.hostname = vagrant_config['domain']
  config.hostmanager.aliases = vagrant_config['aliases']

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "ansible_vagrant", "/vagrant/ansible_vagrant", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=775"]
  config.vm.synced_folder "odoo-addons", "/ops/custom-odoo-addons", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=777,fmode=777"]

  # auto update guest additions
  config.vbguest.auto_update = true

  # provision the vagrant machine using ansible
  # install ansible from its default repo
  config.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.install_mode = :default
      ansible.playbook = "ansible_vagrant/playbook.yml"
      ansible.galaxy_role_file = "ansible_vagrant/requirements.yml"
      ansible.extra_vars = {
        ansible_python_interpreter:"/usr/bin/python3"
      }
  end
end
