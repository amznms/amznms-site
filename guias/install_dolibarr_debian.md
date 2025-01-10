# How to Install Dolibarr ERP on Debian 12

## update the package Index:

```$ sudo apt update```

## install apache web server:

```$ sudo apt install apache2 -y```

## install mariadb database server:

```$ sudo apt install mariadb-server mariadb-client -y```

## install php and required modules:

```$ sudo apt install php libapache2-mod-php php-cli php-intl php-json php-common php-mbstring php-imap php-mysql php-zip php-gd php-mbstring php-curl php-xml -y```

## restart apache2:

```$ sudo service apache2 restart```

## check PHP version

```$ php -v```

## edit php.ini file:

```$ sudo nano /etc/php/8.2/apache2/php.ini```

## change the following settings:

```
memory_limit = 512M
post_max_size = 32M
upload_max_filesize = 32M
date.timezone = America/Manaus
```

## check mariadb service:

```service mariadb status```

## log in to the mariadb console:

```$ sudo mysql -u root -p```

## create the dolibarr database and user:

```
CREATE DATABASE dolibarr;
CREATE USER 'dolibarr'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON dolibarr.* TO 'dolibarr'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## option1: download dolibarr .zip file:

```$ wget https://github.com/Dolibarr/dolibarr/archive/refs/tags/20.0.3.zip```

## extract file .zip

```
$ unzip 19.0.0.zip -d /var/www/
$ mkdir /var/www/dolibarr
$ mv /var/www/dolibarr-20.0.3/htdocs/* /var/www/dolibarr
```

## option2: download dolibarr .tar.gz file:

```$ wget https://github.com/Dolibarr/dolibarr/archive/refs/tags/20.0.3.tar.gz```

## extract file tar.gz

```
$ sudo tar -xzf 19.0.0.tar.gz -C /var/www/
$ mkdir /var/www/dolibarr
$ mv /var/www/dolibarr-20.0.3/htdocs/* /var/www/dolibarr
```

## enable permissions:

```
$ sudo chown -R www-data:www-data /var/www/dolibarr/
$ sudo chmod -R 755 /var/www/dolibarr
```

## configure apache for dolibarr

```$ sudo nano /etc/apache2/sites-available/dolibarr.conf```

## add the following content to the file

```
<VirtualHost *:80>
      ServerName localhost
      DocumentRoot /var/www/html/dolibarr/htdocs
      <Directory /var/www/html/dolibarr/htdocs>
         Options FollowSymLinks
         AllowOverride All
         Require all granted
      </Directory>
      ErrorLog ${APACHE_LOG_DIR}/dolibarr-error.log
      CustomLog ${APACHE_LOG_DIR}/dolibarr-access.log combined
</VirtualHost>
```

## enable the virtual host:

```$ sudo a2ensite /etc/apache2/sites-available/dolibarr.conf```

## restart apache:

```$ sudo service apache2 restart```

## access dolibarr web interface

visit http://localhost

## 000-default.conf

in case of conflict with the default apache page check ```/etc/apache2/sites-available/000-default.conf```

## suggestion:

```
mv /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.bkp
sudo a2ensite /etc/apache2/sites-available/dolibarr.conf
$ sudo service apache2 restart
```

## secure the installation

```$ sudo touch /var/www/html/dolibarr/documents/install.lock```

## dollibar firsts steps:

### Default language:

Select your language and click on the Next step button.

### Prerequisites check:

Validate the PHP checks and click on the Start button.

### web server

Provide your database name, database username, password, admin username and password. Then, click on the Next step button.

### check configuration file:

Installation successful, click on the Next step button.

### check database:

Click on the Next step button.

### Dolibarr admin login

Set a new admin username and password. Then, click on the Next step button.

### End of Setup

Click on the Go to Dolibarr button