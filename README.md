# vagrant-odoo_development

## content

This vagrant machine will install odoo from source to enable you quickly for odoo plugin development. It is based on a few roles, that are solving the dependencies.

## Prequesites

### Maxmind

The odoo installation relies on the maxmind geoip database. You need to create an account with maxmind and set the regarding variables for account_id, license_key and the Edition IDs.

Please follow the instructions on the links:

* https://dev.maxmind.com/geoip/updating-databases
* https://www.wpdiaries.com/country-by-ip/

Afterwards you need to copy the file ansible_vagrant/custom_vars.yml.example to ansible_vagrant/custom_vars.yml and fill in the correct values.

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
* Run the machine

```
vagrant up
```

Vagrant installs odoo 17.0 from source.

### Browse

If you havn't changed the test domains in config.yml and custom_vars.yml, you can open odoo, mailpit and pgadmin4 the following way:

* http://odoo.lokal <- odoo Frontend
* http://www.odoo.lokal <- Additional Domain for use with e.g. website module
* http://shop.odoo.lokal <- Additional Domain for use with e.g. website module
* http://mail.odoo.lokal <- Catch All Email service
* http://pgadmin.odoo.lokal <- Database Frontend

#### Default odoo credentials

Odoo starts with the database selector page, which allows you to create a multitude of tenants/databases. You even can preseed some of them with demo data.

* http://www.odoo.lokal/web/database/selector

Create your first database and start odoo evaluation.

```
Master Password: P@ssw0rd! <- hardcoded in the playbook
Database: odoo_WHATEVER <- set an odoo_ prefixed database name
Email: Your Admin Email <- enter you login email address, e.g. admin@odoo.lokal
Password: Your Admin Password <
Phone number: Some Phone Number
Language: Select your language
Country: Select your country
Preseed with dummy data if desired
```

To login to your tenant, visit the folowing URL and select your database (if more than one DBs are created).

* http://odoo.lokal/web/login

#### pgAdmin4 Database Access

##### Login credentials

The default credentials (hardcoded in the playbook) are:

* Email: admin@pgadmin.odoo.lokal
* Password: admin

##### Database Access

Create new server -> set the name (e.g. odoo)

Tab Connection

* Host: localhost
* Port: 5432
* Maintenance Database: postgres
* Username: odoo
* Password: odoo_secret

These credentials are also hardcoded in the playbook.

### Config

You can generate a list of available odoo config parameters by running the following steps:

```
sudo systemctl stop odoo
sudo su odoo
python3 /opt/odoo/odoo17/odoo-bin --save
```

Depending on the user, running that command, the generated config file can be found in ~/.odoorc

### Module Development

The **demo_module** in folder **custom-odoo-addons** was scaffolded the following way:

```
python3 /opt/odoo/odoo17/odoo-bin scaffold demo_module /opt/custom-odoo-addons/
```

### Customizing

If your host allows to have more resources consumed by the Vagrant machine, you can upgrade the cpu cores and memory in the /config.yaml file. Ich you are changing Domains or other config parameters, you need to make sure, that these are also reflected in the ansible_vagrant/playbook.yml.

e.g.

```
---
configs:
  memory: "4096"
  cpus: "4"
  ansible_version: "latest"
  ip: "192.168.56.12"
  domain: odoo.lokal
  aliases:
  - www.odoo.lokal
  - shop.odoo.lokal
  - mail.odoo.lokal
  - pgadmin.odoo.lokal
...
```

### Debugging

```
tail -f /var/log/odoo/odoo-server.log
systemctl status odoo
journalctl -u odoo.service
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

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get a full-featured [Open Source ERP](https://www.odoo.com) when you install several Apps.

### Getting started with Odoo

* [Setup instructions](https://www.odoo.com/documentation/17.0/administration/install/source.html)
* [Odoo eLearning](https://www.odoo.com/slides)
* [Scale-up](https://www.odoo.com/page/scale-up-business-game)
* [Developer tutorials](https://www.odoo.com/documentation/17.0/developer/tutorials/getting_started.html)

## License

MIT

## Author Information

&copy; Andre Lohmann (and others) 2024

https://github.com/andrelohmann

### Maintainer Contact

* Andre Lohmann
  <lohmann.andre (at) gmail (dot) com>
