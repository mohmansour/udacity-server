# udacity-server

##IP Address 18.197.136.66

##port: 2200/tcp

## URL : http://ec2-18-197-136-66.eu-central-1.compute.amazonaws.com/

## Summary 
1- Create new user named grader and give it the permission to sudo
	`$sudo adduser grader`
	`$sudo nano /etc/sudoers.d/grader` & write in it
2- prevent all users from login with password
	`grader ALL=(ALL) NOPASSWD:ALL`
	`su - grader`
	`mkdir .ssh`
	`touch .ssh/authorized_keys`
3- generate public/private key auth. locally & put public key on server
	`$ssh-keygen` on local terminal
	`$sudo cat .ssh/public_key.pub` & copy its content & put in
	`$sudo nano .ssh/authorized_keys`
	`$sudo chmod 777 .ssh`
	`$sudo chmod 644 .ssh/authorized_keys` 
4- change to port 2200 &modify ufw
	`sudo nano /etc/ssh/sshd_config` & change 22 to 2200
	`sudo ufw default deny incoming`
	`sudo ufw default allow outgoing`
	`sudo ufw allow 2200/tcp`
	`sudo ufw allow 80/tcp`
	`sudo ufw allow 123/udp`
	`sudo ufw allow ntp`
	`sudo ufw allow www`
	`sudo ufw enable`
5-update & modify timezone to UTC
	`sudo apt-get update`
	`sudo apt-get upgrade`
	`sudo dpkg-reconfigure tzdata`
6-login using grader user & public key in /.ssh/authorized_keys
	`ssh grader@18.197.136.66 -p 2200 -i ~/.ssh/authorized_keys`
7-installing apache2
	`sudo apt-get install apache2`
8-install mod_wsgi
	`sudo apt-get install libapache2-mod-wsgi python-dev`
	`sudo a2enmod wsgi`
	`sudo service apache2 start`
8-clone the project on /var/www/catalog/catalog/
	`sudo apt-get install git`
	`cd /var/www`
	`sudo mkdir catalog`
	`cd /catalog`
	`git clone https://github.com/mohmansour/Category-flask.git catalog`
9-create catalog.wsgi & write in it
	`touch catalog.wsgi`
	`nano catalog.wsgi` & write in it
	`import sys
	import logging
	logging.basicConfig(stream=sys.stderr)
	sys.path.insert(0, "/var/www/catalog/catalog")
	from catalog import app as application
	application.secret_key = 'supersecretkey'`
	`mv project.py __init__.py`
9-install virtual environment
 	`sudo apt-get install python-pip`
	`sudo pip install virtualenv`
	`source venv/bin/activate`
	`sudo chmod -R 777 venv`
10-installing all needed libraries
	`pip install Flask`
	`sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils`
11-change pathes of client_secrets.json & fb_client_secret.josn
	`nano __init__.py` & put this path before json files names
	`/var/www/catalog/catalog/client_secrets.json`
12-Configure and enable a new virtual host
	`sudo nano /etc/apache2/sites-available/catalog.conf` & put
	
	`<VirtualHost *:80>
    ServerName 18.197.136.66
    ServerAlias ec2-18-197-136-66.eu-central-1.compute.amazonaws.com
    ServerAdmin admin@18.197.136.66
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>`

	`sudo a2ensite catalog`
	
12-initiate database schema 
	`sudo apt-get install libpq-dev python-dev`
	`udo apt-get install postgresql postgresql-contrib`
	`sudo su - postgres`
	`psql`
	`CREATE USER catalog WITH PASSWORD 'password';`
	`ALTER USER catalog CREATEDB;`
	`CREATE DATABASE catalog WITH OWNER catalog;`
	`\c catalog`
	`REVOKE ALL ON SCHEMA public FROM public;`
	`GRANT ALL ON SCHEMA public TO catalog;`
	`\q`
	`exit`
13-change paths of database engine (from sqlite to postgresl)
	`create_engine('postgresql://catalog:password@localhost/catalog')`
14-setup database & start database entry
	`python /var/www/catalog/catalog/database_setup.py`
	`python /var/www/catalog/catalog/database_entry.py`
15-restart apache
	`sudo service apache2 restart`


## SSH Key Locate:
/.ssh/authorized_keys

##Contents

database_setup.py
database_entry.py
project.py
sport.db
client_secrets.json
fb_client_secret.json
static/styles.css
static/banner.jpg
templates/

##Operation use AWS, VirtualBox,Ubuntu 16.04 TLS , Github , Python 2.7

##Install
you need to install Python2.7 ,VirtualBox & AWS Instance

Credits
this project developed by Mohamed Mansour during the Full Stack Web Developer Nanodegree on Udacity