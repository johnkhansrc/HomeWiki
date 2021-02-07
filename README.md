#LAMP STACK INSTALLATION

```bash
ssh user@razberryIp
sudo apt update
sudo apt upgrade
clear
sudo apt install apache2
sudo ufw app list
```

Ok, we will allow apache services in firewall

```bash
sudo ufw allow in "Apache"
sudo ufw allow in "Apache Full"
sudo ufw allow in "OpenSSH"
sudo ufw allow in "Apache Secure"
```

And now, we can check if they are correctly enabled

```bash
sudo ufw status

```

so get razberryIp ..

```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

then visit razberryIp with 80 port on browser we can show apache default conf page . OK
Now, let's install database ..

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```

Check installation

```bash
sudo mysql
```

Good !
Now, let's install PHP

```bash
sudo apt install php libapache2-mod-php php-mysql
```

OK. Stack is installed, we will create a first VHost ..

```bash
sudo mkdir /var/www/wiki
sudo chown -R $USER:$USER /var/www/wiki
sudo nano /etc/apache2/sites-available/wiki.conf
```

```conf
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/wiki
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

And activate conf

```bash
sudo a2ensite wiki
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

Check if conf is OK whis a basic html file

```bash
nano /var/www/wiki/index.html
```

```html
<h1>Work in progress<h1>
```

Great!
Oour backend will be developed in PHP, so we want set php in main index

```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
sudo systemctl reload apache2
```

Then test if install is right

```bash
nano /var/www/wiki/info.php
```

```php
<?php
phpinfo();
?>
```

```bash
nano /var/www/wiki/index.html
```

```html
<h1>Work in progress</h1>

<a href="./info.php">Show conf</a>
```
Great !!!
Now return in local environnement and initialize symfony
```bash
exit
symfony new --full HomeWiki
cd HomeWiki
git remote add origin myGithubRpo
git push origin master
```


