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

  * Install the latest virtualbox from oracle repositories (https://www.virtualbox.org/wiki/Downloads)
  * If you are on a linux distro, follow the instructions to add the oracle repo
  * Install the latest Oracle VM VirtualBox Extension Pack

### Vagrant

#### cli

  * Install the latest vagrant (https://www.vagrantup.com/downloads.html)

#### plugins

The vagrant machines depends on the following vagrant plugins.

```
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-hostmanager
```

These plugins should get installed automatically on a "vagrant up", if that fails anyways, please manually install the plugins by entering the commands.

## usage

### upstart

  * Clone the repo and change to the directory
  * Copy config.yml.example to config.yml and set your custom configurations
  * Copy ansible_vagrant/custom_vars.yml.example to ansible_vagrant/custom_vars.yml and set your custom vars
  * Run the machine

```
vagrant up
```

Vagrant installs odoo 13.0 fro source and initializes the database with a set of demo data.

### Browse

If you havn't changed the test domains in config.yml and custom_vars.yml, you can open odoo and mailhog the following way:

  * http://odoo.lokal
  * http://mail.lokal

#### Default credentials

  * Email: admin
  * Password: admin

### Config

The **odoo13.conf.template** was generated the following way:

```
python3 /opt/odoo/odoo13/odoo-bin --save
```

Depending on the user, running that command, the generated config file can be found in ~/.odoorc

### Module Development

The **demo_module** in folder **custom-odoo-addons** was scaffolded the following way:

```
python3 /opt/odoo/odoo13/odoo-bin scaffold demo_module /opt/custom-odoo-addons/
```

## Odoo

Odoo is a suite of web based open source business apps.

The main Odoo Apps include:
  * [Open Source CRM](https://www.odoo.com/page/crm)
  * [Website Builder](https://www.odoo.com/page/website-builder)
  * [eCommerce](https://www.odoo.com/page/e-commerce)
  * [Warehouse Management](https://www.odoo.com/page/warehouse)
  * [Project Management](https://www.odoo.com/page/project-management)
  * [Billing & Accounting](https://www.odoo.com/page/accounting)
  * [Point of Sale](https://www.odoo.com/page/point-of-sale)
  * [Human Resources](https://www.odoo.com/page/employees)
  * [Marketing](https://www.odoo.com/page/lead-automation)
  * [Manufacturing](https://www.odoo.com/page/manufacturing)
  * [...](https://www.odoo.com/#apps)

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured [Open Source ERP](https://www.odoo.com) when you install several Apps.


### Getting started with Odoo

  * [Setup instructions](https://www.odoo.com/documentation/13.0/setup/install.html)
  * [Odoo eLearning](https://www.odoo.com/slides)
  * [Scale-up](https://www.odoo.com/page/scale-up-business-game)
  * [Developer tutorials](https://www.odoo.com/documentation/13.0/tutorials.html)
