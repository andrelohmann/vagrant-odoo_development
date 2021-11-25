# -*- mode: ruby -*-
# vi: set ft=ruby :

# load configs
require 'yaml'
current_dir    = File.dirname(File.expand_path(__FILE__))
configs        = YAML.load_file("#{current_dir}/config.yml")
vagrant_config = configs['configs']

# require necessary plugins
required_plugins = %w( vagrant-hostmanager vagrant-vbguest )
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|

  # activate hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  config.vm.box = "ubuntu/focal64"
  #config.vm.box = "ubuntu/bionic64"

  # set memory to 2048m
  config.vm.provider "virtualbox" do |vb|
    vb.memory = vagrant_config['memory']
    vb.cpus = vagrant_config['cpus']
    vb.customize ["modifyvm", :id, "--audio", "none"]
  end

  # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
  config.vm.network "private_network", ip: vagrant_config['ip']
  config.vm.hostname = vagrant_config['domain']
  config.hostmanager.aliases = vagrant_config['aliases']

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "ansible_vagrant", "/vagrant/ansible_vagrant", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=775,fmode=775"]
  config.vm.synced_folder "custom-odoo-addons", "/opt/custom-odoo-addons", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=777,fmode=777"]

  # auto update guest additions
  config.vbguest.auto_update = true

  # provision the vagrant machine using ansible
  # install ansible from its default repo
  config.vm.provision "shell", inline: "test -f .ssh/authorized_keys && cp --preserve=all .ssh/authorized_keys /tmp/authorized_keys" # preserve authorized_keys for `vagrant ssh` in case it already exists
  config.vm.provision "file", source: "~/.ssh", destination: ".ssh" # copy any key from the host; it is not possible to copy them as id_*, so other files might get overriden
  config.vm.provision "shell", inline: "mv /tmp/authorized_keys .ssh/" # restore any prior file

  # Run Ansible from the Vagrant VM
  if config.vm.box == "ubuntu/focal64"
    # install ansible from default repo
    config.vm.provision "shell", inline: "apt update && apt install ansible -y"
    config.vm.provision "ansible_local" do |ansible|
      ansible.install = false
      ansible.playbook = "ansible_vagrant/playbook.yml"
      ansible.galaxy_role_file = "ansible_vagrant/requirements.yml"
      #Uncomment when ansible 2.10 is available
      #ansible.galaxy_command = "sudo ansible-galaxy install -r %{role_file} --force; sudo ansible-galaxy collection install -r %{role_file} --force"
      ansible.extra_vars = {
        ansible_python_interpreter:"/usr/bin/python3"
      }
    end
  else
    config.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.install_mode = :default
      ansible.playbook = "ansible_vagrant/playbook.yml"
      ansible.galaxy_role_file = "ansible_vagrant/requirements.yml"
      #Uncomment when ansible 2.10 is available
      #ansible.galaxy_command = "sudo ansible-galaxy install -r %{role_file} --force; sudo ansible-galaxy collection install -r %{role_file} --force"
      ansible.extra_vars = {
        ansible_python_interpreter:"/usr/bin/python3"
      }
    end
  end

  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true
end
