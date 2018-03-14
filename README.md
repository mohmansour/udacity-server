# udacity-server

##IP Address 18.197.136.66

##port: 2200/tcp

## URL : http://ec2-18-197-136-66.eu-central-1.compute.amazonaws.com/

## Summary 
1- Create new user named grader and give it the permission to $sudo <br/>
	`$sudo adduser grader` <br/>
	`$sudo nano /etc/$sudoers.d/grader` & write in it <br/>
2- prevent all users from login with password <br/>
	`grader ALL=(ALL) NOPASSWD:ALL` <br/>
	`su - grader` <br/>
	`mkdir .ssh`<br/>
	`touch .ssh/authorized_keys`<br/>
3- generate public/private key auth. locally & put public key on server<br/>
	`$ssh-keygen` on local terminal<br/>
	`$sudo cat .ssh/public_key.pub` & copy its content & put in<br/>
	`$sudo nano .ssh/authorized_keys`<br/>
	`$sudo chmod 777 .ssh`<br/>
	`$sudo chmod 644 .ssh/authorized_keys`<br/> 
4- change to port 2200 &modify ufw<br/>
	`$sudo nano /etc/ssh/sshd_config` & change 22 to 2200<br/>
	`$sudo ufw default deny incoming`<br/>
	`$sudo ufw default allow outgoing`<br/>
	`$sudo ufw allow 2200/tcp`<br/>
	`$sudo ufw allow 80/tcp`<br/>
	`$sudo ufw allow 123/udp`<br/>
	`$sudo ufw allow ntp`<br/>
	`$sudo ufw allow www`<br/>
	`$sudo ufw enable`<br/>
5-update & modify timezone to UTC<br/>
	`$sudo apt-get update`<br/>
	`$sudo apt-get upgrade`<br/>
	`$sudo dpkg-reconfigure tzdata`<br/>
6-login using grader user & public key in /.ssh/authorized_keys<br/>
	`ssh grader@18.197.136.66 -p 2200 -i ~/.ssh/authorized_keys`<br/>
7-installing apache2<br/>
	`$sudo apt-get install apache2`<br/>
8-install mod_wsgi<br/>
	`$sudo apt-get install libapache2-mod-wsgi python-dev`<br/>
	`$sudo a2enmod wsgi`<br/>
	`$sudo service apache2 start`<br/>
8-clone the project on /var/www/catalog/catalog/<br/>
	`$sudo apt-get install git`<br/>
	`cd /var/www`<br/>
	`$sudo mkdir catalog`<br/>
	`cd /catalog`<br/>
	`git clone https://github.com/mohmansour/Category-flask.git catalog`<br/>
9-create catalog.wsgi & write in it<br/>
	`touch catalog.wsgi`<br/>
	`nano catalog.wsgi` & write in it<br/>
	`import sys  `<br/>
	`import logging`  <br/>
	`logging.basicConfig(stream=sys.stderr)`  <br/>
	`sys.path.insert(0, "/var/www/catalog/catalog")`  <br/>
	`from catalog import app as application  `<br/>
	`application.secret_key = 'supersecretkey'`  <br/>
	`mv project.py __init__.py`<br/>
9-install virtual environment<br/>
 	`$sudo apt-get install python-pip` <br/>
	`$sudo pip install virtualenv`<br/>
	`source venv/bin/activate`<br/>
	`$sudo chmod -R 777 venv`<br/>
10-installing all needed libraries<br/>
	`pip install Flask`<br/>
	`$sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils`<br/>
11-change pathes of client_secrets.json & fb_client_secret.josn<br/>
	`nano __init__.py` & put this path before json files names<br/>
	`/var/www/catalog/catalog/client_secrets.json`<br/>
12-Configure and enable a new virtual host<br/>
	`$sudo nano /etc/apache2/sites-available/catalog.conf` & put<br/>
	
	`<VirtualHost *:80> `
<br/>
    `ServerName 18.197.136.66` <br/>
    `ServerAlias ec2-18-197-136-66.eu-central-1.compute.amazonaws.com` <br/>
    `ServerAdmin admin@18.197.136.66 `<br/>
    `WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages` <br/>
    `WSGIProcessGroup catalog` <br/>
    `WSGIScriptAlias / /var/www/catalog/catalog.wsgi` <br/>
    `<Directory /var/www/catalog/catalog/>` <br/>
        `Order allow,deny` <br/>
        `Allow from all` <br/>
    `</Directory> `<br/>
    `Alias /static /var/www/catalog/catalog/static `<br/>
    `<Directory /var/www/catalog/catalog/static/>` <br/>
        `Order allow,deny` <br/>
     `   Allow from all` <br/>
    `</Directory> `<br/>
    ErrorLog ${APACHE_LOG_DIR}/error.log <br/>
    `LogLevel warn` <br/>
    `CustomLog ${APACHE_LOG_DIR}/access.log combined` <br/>
	`</VirtualHost>` <br/>

13-enable virtual env
	`$sudo a2ensite catalog` <br/>
	
14-initiate database schema <br/>
	`$sudo apt-get install libpq-dev python-dev`<br/>
	`udo apt-get install postgresql postgresql-contrib`<br/>
	`$sudo su - postgres`<br/>
	`psql`<br/>
	`CREATE USER catalog WITH PASSWORD 'password';`<br/>
	`ALTER USER catalog CREATEDB;`<br/>
	`CREATE DATABASE catalog WITH OWNER catalog;`<br/>
	`\c catalog`<br/>
	`REVOKE ALL ON SCHEMA public FROM public;`<br/>
	`GRANT ALL ON SCHEMA public TO catalog;`<br/>
	`\q`<br/>
	`exit`<br/>
15-change paths of database engine (from sqlite to postgresl)<br/>
	`create_engine('postgresql://catalog:password@localhost/catalog')`<br/>
16-setup database & start database entry<br/>
	`python /var/www/catalog/catalog/database_setup.py`<br/>
	`python /var/www/catalog/catalog/database_entry.py`<br/>
17-restart apache<br/>
	`$sudo service apache2 restart`<br/>


## SSH Key Locate:
/.ssh/authorized_keys

##Contents

database_setup.py <br/>
database_entry.py <br/>
project.py <br/>
sport.db <br/>
client_secrets.json <br/>
fb_client_secret.json <br/>
static/styles.css <br/>
static/banner.jpg <br/>
templates/ <br/>

##Operation use AWS, VirtualBox,Ubuntu 16.04 TLS , Github , Python 2.7

##Install
you need to install Python2.7 ,VirtualBox & AWS Instance

Credits
this project developed by Mohamed Mansour during the Full Stack Web Developer Nanodegree on Udacity