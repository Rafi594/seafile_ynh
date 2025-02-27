Seafile for Yunohost
============

Seafile is an open Source Cloud Storage application.

Official website: <http://seafile.com/>

Requirements
------------

Functionnal instance of [Yunohost](https://yunohost.org/#/)

Installation
------------

From command line:

`sudo yunohost app install -l Seafile https://github.com/YunoHost-Apps/seafile_ynh`

Upgrade
-------

From command line:

`sudo yunohost app upgrade -u https://github.com/YunoHost-Apps/seafile_ynh`

Infos
-----

Seafile server v6.0.6

Available for x64, i386, and Raspberry Pi architecture but only tested for x64 (feedback are welcome)

Seafile no longer distribute binary for generic armhf architectures but rpi binary could work.

/!\ To login use your yunohost email not your username.

License
-------

Seafile server and its desktop clients are published under GPLv2.

Mobile clients are published under the GPLv3.

The Seafile server's web end, i.e. Seahub, is published under the Apache License.

This package is published under MIT License

TODO
-----

 - Auto login/logout, see #1
 - Test of backup/restore script

Use a spial user and put seafile binary in /opt dir :
--------------------------------------

With this new package for a better security, it's possible to run seafile with a special user (seafile) put all seafile file in /opt/yunohost dir.
To do this open a console and do this command :

```
# stop seafile server
sudo service seafile-server stop

# Move all data to opt and change user
sudo mv /var/www/seafile /opt/yunohost/seafile
sudo addgroup seafile --system --quiet
sudo adduser seafile --disabled-login --ingroup seafile --system --quiet --shell /bin/bash --home /opt/yunohost/seafile

# Adapt configuration
sudo sed -i "s@user=www-data@user=seafile@g" /etc/init.d/seafile-server 
sudo sed -i "s@seafile_dir=/var/www/seafile@seafile_dir=/opt/yunohost/seafile@g" /etc/init.d/seafile-server
sudo sed -i "s@alias /var/www/seafile/@alias /opt/yunohost/seafile/@g" /etc/nginx/conf.d/$(sudo yunohost app setting seafile domain).d/seafile.conf

# Set the good user for seafile
sudo chown seafile:seafile -R /opt/yunohost/seafile
sudo chown seafile:seafile -R /home/yunohost.app/seafile-data/

# Restart services
sudo rm -rf /tmp/seahub_cache
sudo systemctl daemon-reload
sudo service nginx reload
sudo service seafile-server start
```

Developper infos
----------------

Please do your pull request to the Dev branch.

Test or upgrade to dev version:
```
su - admin
git clone -b Dev https://github.com/YunoHost-Apps/seafile_ynh
# to install
sudo yunohost app install -l Seafile /home/admin/seafile_ynh
# to upgrade
sudo yunohost app upgrade -f /home/admin/seafile_ynh seafile

```
