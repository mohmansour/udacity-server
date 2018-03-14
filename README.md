# udacity-server

##IP Address 18.197.136.66

##port: 2200/tcp

## URL : http://ec2-18-197-136-66.eu-central-1.compute.amazonaws.com/

## Summary 
1- Create new user named grader and give it the permission to sudo
2- prevent all users from login with password
3- generate public/private key auth. locally & put public key on server
4- login using grader user & public key in /.ssh/authorized_keys
5-modify ufw (deny all incoming & allow all outgoing then allow some ports specally 2200/tcp)
6-installing apache2
7-installing all needed libraries (flask, python-pip , psycopg2,...)
8-clone the project on /var/www/catalog/catalog/
9-start a virtual environment & call all libraries in it
10-change pathes of client_secrets.json & fb_client_secret.josn
11-change paths of database engine (from sqlite to postgresl)
12-initiate database schema 
13-setup database & start database entry

## SSH Key:
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcYEkX5irrXACLE9Di+QZ5SAzKnOHBkDO/q4abWfkxKShWWXOjn6nr+Aetrud5xIp7JkHsJamFmQtoNbxPEg4FdTcrJfwk5WzKfylXf/giiG7jLpKh1yeVoPl91FB+/Uxda1MiTIdvHHxPJZMhQ3uD+257Qr9fi5ogVuQrSBXdgvTOUfq5au700YGeIE48F+xEqRDRnodIbPzr0+DH5AUYLhXCFFlrfvQUykJG3ak36U0cnygEw8XOuRG9cGxt1JxiDgVYCbtfUQ3/8ss+th6d9z+PWsoMxODcQ85hayaj/gHfVBF+qt0tJnaV5xEaY8SnTmOg+Jf44zso/NXM28k7 momhmansour@momhmansour-VirtualBox

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