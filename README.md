# vagrant-odoo_development

(c) Andre Lohmann (and others) 2020

## Maintainer Contact
 * Andre Lohmann
   <lohmann.andre (at) gmail (dot) com>

## content

This vagrant machine will install odoo from sources to enable you quickly for odoo plugin development.
The install process is therefor following the install description found [here](https://www.odoo.com/documentation/13.0/setup/install.html#id7).

## Prequesites

### VirtualBox

  * install the latest virtualbox from oracle repositories (https://www.virtualbox.org/wiki/Downloads)
  * if you are on a linux distro, follow the instructions to add the oracle repo
  * install the latest Oracle VM VirtualBox Extension Pack

### Vagrant

#### cli

  * Install the latest vagrant (https://www.vagrantup.com/downloads.html)

#### plugins

the vagrant machines depends on the following vagrant plugin.

```
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-hostmanager
```

this pluin should get installed automatically on a "vagrant up", if that fails anyways, please manually install the plugin by entering the command

## usage

### upstart

  * clone the repo and change to the directory
  * copy config.yml.example to config.yml and set your custom configurations
  * copy ansible_vagrant/custom_vars.yml.example to ansible_vagrant/custom_vars.yml and set your custom vars
  * run the machine

```
vagrant up
```

### Browse

If you havn't changed the test domains in config.yml and custom_vars.yml, you can open odoo and mailhog the following way:

  * http://odoo.lokal
  * http://mail.lokal

### Module Development

The **demo_module** in folder **odoo-addons** was scaffolded the following way:

```
python3 /opt/odoo/odoo-bin scaffold demo_module /opt/custom-odoo-addons/
```
