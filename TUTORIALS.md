# Tutorials

## Debugging odoo

```
tail -f /var/log/odoo/odoo.log
systemctl status odoo
journalctl -u odoo.service
```

## Create a Database

You can setup a test tenant using odoo online, following [this tutorial](https://www.odoo.com/de_DE/slides/slide/create-an-odoo-database-714?fullscreen=1), which requires an upfront registration to odoo (without the necessity to create a payed account).

On our local community edition setup do the following:

  * vagrant up the machine
  * http://odoo.lokal <- login
  * upper left corner (the 9 tiles) -> Settings -> Users & Companies -> Companies
  * Edit your Company Data

Creating a second database requires the odoo user to have the regarding priveledges (role_attr_flags: CREATEDB,NOSUPERUSER <- the the README.md).

  * when logged in - upper right corner (Mitchel Admin)
  * log out
  * http://odoo.lokal/web/login
  * Manage Databases
  * Create Database

```
Master Password: P@ssw0rd!
Database: odoo_WHATEVER
Email: Your Admin Email
Password: Your Admin Password
Phone number
Language
Country
Preseed with dummy data if desired
```
