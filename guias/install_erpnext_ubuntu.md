# How to Install ERPNext Version 14 in Ubuntu 20.04

```
sudo apt-get update
```

```
sudo adduser [frappe-user]
usermod -aG sudo [frappe-user]
su [frappe-user]
cd /home/[frappe-user]
```

ubuntu 24 or debian 12

```
sudo adduser [frappe-user] sudo
su [frappe-user]
cd /home/[frappe-user]
```

## Install Required Packages

```
sudo apt-get install git

sudo apt-get install python3-dev python3.10-dev python3-setuptools python3-pip python3-distutils

sudo apt-get install python3.10-venv

sudo apt-get install software-properties-common

sudo apt install mariadb-server mariadb-client

sudo apt-get install redis-server

sudo apt-get install xvfb libfontconfig wkhtmltopdf sudo apt-get install libmysqlclient-dev

```

libmysqlclient-dev or default-libmysqlclient-dev

## Configure MYSQL Server

```
sudo mysql_secure_installation
```

Enter current password for root: (Enter your SSH root user password)

Switch to unix_socket authentication [Y/n]: Y

Change the root password? [Y/n]: Y

It will ask you to set new MySQL root password at this step. This can be different from the SSH root user password.

Remove anonymous users? [Y/n] Y

Disallow root login remotely? [Y/n]: N

This is set as N because we might want to access the database from a remote server for using business analytics software like Metabase / PowerBI / Tableau, etc.

Remove test database and access to it? [Y/n]: Y

Reload privilege tables now? [Y/n]: Y

## Edit MYSQL default config file

```
sudo nano /etc/mysql/my.cnf
```

```
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
[mysql]
default-character-set = utf8mb4
```

em outras situacoes:

[copiar codigo do repo](https://github.com/karani-gk/ERPNext_mariadb_conf)
substitua no arquivo seguinte:

```sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf```

e depois:

```sudo systemctl restart mariadb```

## Instal CURL, Node, NPM and Yarn

```
sudo apt install curl

curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

source ~/.bashrc

nvm install 18

sudo apt-get install npm

sudo npm install -g yarn

sudo pip3 install frappe-bench
```

em outras situacoes:

```sudo apt install pipx```

```pipx install frappe-bench```

caso **bench** nao estiver no path, verifique

```ls ~/.local/bin```
or
```ls /root/.local/bin```

```
nano ~/.bashrc
```

```
export PATH=$PATH:~/.local/bin
```

```
source ~/.bashrc
```

a partir de agora esteja em:

``` cd ~/```

e /home/[frappe-user]

## Install Frappe Bench

```
bench init --frappe-branch version-14 frappe-bench

cd frappe-bench

chmod -R o+rx /home/[frappe-user]
```

## Create a New Site

```bench new-site [site-name]```

## Install ERPNext and other Apps

```
bench get-app payments

bench get-app --branch version-14 erpnext
```

## Install all the apps on our site

```
bench --site [site-name] install-app erpnext
```

## Install all the other apps

```
bench get-app hrms --branch version-14

bench --site [site-name] install-app hrms
```

O **hrms** pode listar erro de conexao com o redis-server
isso e normal na maioria dos casos, ignore e prossiga

## Evite o erro 404

```nano sites/currentsite.txt```

insira somente na primeira linha:

```yoursitename```

## Execute

```bench start```

por padrao:
[SERVER IP:8000]

[localhost:8000](http://localhost:8000)


## outras situacoes: troca de versao nodejs

```nvm install 20```

Se voce criar outro usuario para ubuntu/debian e abrir sua sessao, talvez precisara adicionar:

```nano ~/.bashrc```

```
export NVM_DIR=path/to/current/user/.nvm
```

```source ~/.bashrc```

ai podera usar no novo usuario:

```curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash```

## outras versoes do python

as vezes o SO tem uma versao antiga, voce pode instalar varias versoes, porem phython e python3 tem link somente para antiga
e as instalacoes do bench e erpnext irao usar estes nomes

### Add custom APT repository

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
```

instale python 3.10

```
sudo apt install python3.10 python3.10-venv python3.10-dev
python3 --version
```

se a versao e antiga:

```
ls -la /usr/bin/python3
sudo rm /usr/bin/python3
sudo ln -s python3.10 /usr/bin/python3
python3 --version
```

### Install PIP for Python 3.10

```
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10
python3.10 -m pip --version
```

se ultimo acima nao funcionar tente:

```curl https://bootstrap.pypa.io/get-pip.py | sudo python3```


### APAGAR UM BANCO DE DADOS (CUIDADO)

toda vez que cria um ```branch new-site sitename``` vai solicitar acesso ao root mysql e criar um banco
o nome do banco e baseado no nome do site, caso voce excluir o diretorio em sites/yoursitename
ao recriar o site, antes deveras apagar o banco antigo fazendo:

```
mysql -u root -p

show databases;
drop database id_do_banco;
exit;
```

### MariaDB denied permission

se ```bench new-site yousitename``` der erro, tente:

```
sudo mysql -u root
mysql> SHOW DATABASES;
mysql> USE mysql;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE 
User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;

sudo service mysql restart
```

# modules

## lista site1:

Install HR and Payroll
```
$  bench get-app hrms
$  bench --site default install-app hrms
```
Install Healthcare
```
$  bench get-app healthcare
$  bench --site default install-app healthcare
```
Install Education
```
$  bench get-app education
$  bench --site default install-app education
```
Install E-commerce Integration
```
$  bench get-app ecommerce_integrations  --branch develop
$  bench --site default install-app ecommerce_integrations
```
Install Hospitality
```
$  bench get-app hospitality
$  bench --site default install-app hospitality
```
Install Non-Profit
```
$  bench get-app non_profit
$  bench --site default install-app non_profit
```
Install Agriculture
```
$  bench get-app agriculture
$  bench --site default install-app agriculture
```
Install Frappe Chat
```
$  bench get-app chat
$  bench --site default install-app chat
```
Install Frappe Wiki
```
$  bench get-app wiki https://github.com/frappe/wiki --branch version-14
$  bench --site default install-app wiki
```
Install POS-Awesome
```
$  bench get-app https://github.com/yrestom/POS-Awesome.git
$  bench setup requirements
$  bench --site default install-app posawesome
```

## lista site2:

```
bench get-app library  https://github.com/frappe/library_management.git
```

```
bench --site [site-name] install-app library_management
```

## lista site3

-----For ERPNext Module-----
```
bench get-app erpnext --branch version-15
bench --site mysite.com install-app erpnext
```
-----For Payments Module-----
```
bench get-app payments --branch version-15
bench --site mysite.com install-app payments
```
-----For HRMS Module-----
```
bench get-app hrms --branch version-15
bench --site mysite.com install-app hrms
```
-----For CRM Module-----
```
bench get-app CRM
bench --site mysite.com install-app CRM
```

atencao: CRM ja inclusa na 14, nao precisa instalar


## site4

[webshop](https://github.com/frappe/webshop)

```
$ bench get-app webshop

$ bench --site sitename install-app webshop
```


eu vi por ai
[modules.txt develop](https://github.com/frappe/erpnext/blob/develop/erpnext/modules.txt)

```
Accounts
CRM
Buying
Projects
Selling
Setup
Manufacturing
Stock
Support
Utilities
Assets
Portal
Maintenance
Regional
ERPNext Integrations
Quality Management
Communication
Telephony
Bulk Transaction
Subcontracting
EDI
```


[GUIA ERPNEXT ESPANHOL](https://github.com/sihaysistema/Guia-ERPNext/blob/master/Aprendiendo-a-usar-ERPNext.md)

[GUIA ERPNEXT INGLES](https://github.com/sihaysistema/ERPNext-Guide/blob/master/ERPNext-Learning-Guide.md)


# PRODUCTION


## instalar alguns app

```
sudo apt-get install fail2ban```
```
```
sudo apt-get install nginx
```

configure o site:

```
bench --site [site-name] enable-scheduler
```
```
bench --site [site-name] set-maintenance-mode off
```
```
sudo bench setup production [frappe-user]
```

```
bench setup nginx
```

```
sudo supervisorctl restart all sudo bench setup production [frappe-user]
```

Agora voce pode acessar o seu site erpnext pela porta :80


## Certificado LetsEncrypt

```
sudo service nginx stop 
```
```
sudo apt install certbot -y 
```
```
sudo apt install certbot python3-certbot-nginx 
```
```
sudo certbot certonly -a nginx -d yoursite.example.com 
```

Se ocorreu tudo certo, seu certificado esta em ```/etc/letsencrypt/live/```

algumas mudancas devem serem feitas em /etc/nginx/conf.d/frappe-bench.conf

```

server{
    listen 443 ssl;

    server_name
        yoursite.example.com
        ;

    root /home/frappeuser/frappe-bench/sites;

    ssl_certificate   /etc/letsencrypt/live/yoursite.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yoursite.example.com/privkey.pem;
    ssl_session_timeout 5m;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers EECDH+AESGCM:EDH+AESGCM;
    ssl_ecdh_curve secp384r1;
    ssl_prefer_server_ciphers on;
}

```
sudo nano /etc/nginx/nginx.conf
```

na secao http

```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
```

## Colocar Segundo Site

Em frappe-bench/ crie o novo site e instale erpnext e modulos

```
bench new-site site2.example.com

bench --site site2.example.com install-app erpnext
```

gere o certificado
```
sudo certbot certonly -a nginx -d site2.example.com
```

faca uma copia e altere os dados:

```
sudo cp /etc/nginx/conf.d/frappe-bench.conf /etc/nginx/conf.d/site2.example.com.conf 
```
```
sudo nano /etc/nginx/conf.d/site2.example.com.conf
```
altere algumas linhas
```
#upstream frappe-bench-frappe {
#   server 127.0.0.1:8000 fail_timeout=0;
#}

#upstream frappe-bench-socketio-server {
#   server 127.0.0.1:9000 fail_timeout=0;
#}
```
```
listen 443 ssl;
```

salve, depois reinicialize:

```
sudo service supervisor restart

sudo service nginx restart
```


# author

Aldson Diego

instagran: @aldsondiego

