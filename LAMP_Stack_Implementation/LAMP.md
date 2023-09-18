# LAMP STACK IMPLEMENTATION
The word LAMP refers to a group of open source software installed together which can be used to host web applications.
***LAMP*** meaning Linux; a unix operating system, Apache; a web server, Mysql; a database and PHP; an Hypertext preprocessor.

### Tools Used For Completion of the project
- An AWS account with an ubuntu EC2 instance
- Virtual machine using linux
- [SSH](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)

## Installation of Apache
update apt repositories

    sudo apt update
Install apache web server

    sudo apt install apache2
Allow firewall for apache

    sudo ufw allow in "Apache"
NOTE: The step above will allow firewall for apache once it is enabled, it is important we allow firewall for ssh. ssh runs on port 22, if firewall is not allowed for ssh running on port 22, connection to the ubuntu instance via ssh will be permanently

    sudo ufw allow 22
    
To check if ubuntu instance has been installed successfully

    sudo systemctl status apache2

To access your web server on your browser

    http://ubuntu_instance_public_ip_address
An Apache default page will displayed. 
You can check your ubuntu instance ip address from your aws console from the EC2 instance service management or input the command below

    curl http://icanhazip.com

## Installation of Mysql

In the previous step, apache web server was installed successfully. In this step, mysql will be installed as a database to store data for our web application.
apt repositories has been updated in the previous step, mysql should be installed directly

    sudo apt install mysql-server
Secure mysql installation 

    sudo mysql_secure_installation
Check if mysql has been successfully installed

    sudo systemctl status mysql
Log into mysql as the root user

    sudo mysql
Log out of mysql

    mysql>exit

## Installing PHP

So far, Apache and Mysql has been installed. The last software of the LAMP stack which is PHP will be installed in this step

    sudo apt install php libapache2-mod-php php-mysql
Check for the successful installation of php

    php -v

## Configuring Apache Web Server To Serve As A Virtual Host 
Create a directory for our codes to be hosted at the location "/var/www/html/darey.io", "darey.io" can be named any name. The directory will contain the php codes which apache will serve. The codes are not limited to php codes but also html, css, javascript e.t.c. . Apache web server is smart enough to know this location and serve it with the help of its configuration file.

    sudo mkdir /var/www/html/darey.io


 darey.io is the directory created which will contain our php code

    
