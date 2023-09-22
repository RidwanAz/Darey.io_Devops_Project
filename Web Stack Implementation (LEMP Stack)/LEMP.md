# LEMP STACK IMPLEMENTATION
In project_2 LAMP Stack was implemented. In the project LEMP Stack will be implemented.
The word LEMP refers to a group of open source software installed together which can be used to host web applications.
***LEMP*** meaning Linux; a unix operating system, Nginx; a web server capable of load balancing and can serve as a reverse proxy, Mysql; a database and PHP; an Hypertext preprocessor.
What makes a LAMP stack different  from LEMP Stack is the Nginx web server which is the most widely used web server.
### Tools Used For Completion of the project
- An AWS account with an ubuntu EC2 instance
- Virtual machine using linux
- [SSH](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)(not necessary)

## Installation of Nginx
update apt repositories

    sudo apt update
Install nginx web server

    sudo apt install nginx
Allow firewall for nginx

    sudo ufw allow in "Nginx HTTP"
NOTE: The step above will allow firewall for nginx on port 80, to allow firewall for nginx on port 443 or any other port, "Nginx HTTPS and Nginx Full" need to be opened. Once it is enabled, it is important we allow firewall for ssh. ssh runs on port 22, if firewall is not allowed for ssh running on port 22, connection to the ubuntu instance via ssh will be permanently denied 

    sudo ufw allow 22
    
To check if nginx web server has been installed successfully

    sudo systemctl status nginx
![nginx](images/Nginx.jpg)


To access your web server on your browser

    http://ubuntu_instance_public_ip_address
![nginx](images/nginx_default.png)


The Nginx default page above will displayed. 
You can check your ubuntu instance ip address from your aws console from the EC2 instance service management or input the command below

    curl http://icanhazip.com
or

    curl -4 icanhazip.com 
## Installation of Mysql

In the previous step, nginx web server was installed successfully. In this step, mysql will be installed as a database to store data for our web application.
apt repositories has been updated in the previous step, mysql should be installed directly

    sudo apt install mysql-server
Secure mysql installation 

    sudo mysql_secure_installation
Check if mysql has been successfully installed

    sudo systemctl status mysql 

![mysql](images/Mysql.jpg)
Log into mysql as the root user

    sudo mysql
Log out of mysql

    mysql>exit

## Installing PHP

So far, Nginx and Mysql has been installed. The last software of the LAMP stack which is PHP will be installed in this step.

The installation of php is different for nginx is different from apache because, the way it interacts with the web server differs. PHP for Apache typically uses the Apache module called mod_php. This module integrates PHP directly into the Apache web server, allowing it to handle PHP requests internally. For Nginx, PHP is typically run as a separate process using FastCGI (e.g., PHP-FPM) or as a reverse proxy. This means Nginx communicates with the PHP interpreter over a network socket or through a unix domain socket.

    sudo apt install php-fpm php-mysql
Check for the successful installation of php

    php -v

## Configuring Nginx Web Server To Serve As A Virtual Host 
Create a directory for our codes to be hosted at the location "/var/www/html/darey.io", "darey.io" can be named any name. The directory will contain the php codes which nginx will serve. The codes are not limited to php codes but also html, css, javascript e.t.c. . Nginx web server is smart enough to know this location and serve it with the help of its configuration file.

    sudo mkdir /var/www/html/darey.io
Create an simple html file which Nginx will serve

    sudo nano /var/www/darey.io/index.html
It should have the content below


    <h1>Welcone to Darey.io, Nginx works</h1>
darey.io is the directory created which will contain our php code
Assign ownership of the directory with the user

    sudo chown -R $USER:$USER /var/www/html/darey.io

Create a new server block configuration that will replace nginx default server block configuration at /etc/apache2/sites-available

    sudo nano /etc/nginx/sites-available/darey.io

darey_io should contain the below

    server {
    listen 80;
    server_name ;
    root /var/www/html/darey.io;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
    }
Unlike apache, nginx only uses the server block configuration file in sites-enabled, sites-available only contains the different server block configuration.
To activate a server block, you create a symbolic link from the sites-enabled directory to the configuration file in the sites-available directory. This link tells Nginx to use that configuration.

Creating a symbolic link 

    sudo ln -s /etc/nginx/sites-available/darey_io /etc/nginx/sites-enabled/

Unlinking the default server block configuration file from sites enables

    sudo rm /etc/nginx/sites-enabled/default
or

    sudo unlink /etc/nginx/sites-enabled/default

Reload nginx conf. file to make sure the file has no errors

    sudo nginx -t

Reload nginx

    sudo systemctl reload nginx

Check your web browser.

    http://ubuntu_instance_public_ip_address


## Testing PHP with Nginx 

Replace the index.html file in /var/www/html/darey.io with index.php with a simple php info.

    sudo nano /var/www/darey.io/html/index.php
Paste the contents below

    <?php
    phpinfo();
Save and close the file 
You can use ubuntu instance public ip address to access the php file served by nginx from your web browser 
![php](images/phpinfo.png)
The default page above will be displayed.

## Testing PHP and Mysql with Nginx (LEMP Stack)

In the previous step, php was tested with nginx by using nginx to serve a php file. In the step mysql database will be connected to php and web served by nginx.
The first to do is to create a database with datas

Log into mysql as root user 

    sudo mysql 

***In the mysql shell***
Create a database called darey_io

    CREATE DATABASE darey_io;

Create a new user called ***darey*** with password ***A different username or password can be used***

    CREATE USER 'darey'@'%' IDENTIFIED WITH mysql_native_password BY 'Ab123456789';
Grant 'darey' all permissions 

    GRANT ALL ON root.* TO 'darey'@'%';
Log out of mysql

    exit
Log in to mysql as user 'darey'

    mysql -u darey -p Aa123456789

Checkout the database created as the root user

    SHOW DATABASES;
The output below will be printed

    +--------------------+
    | Database           |
    +--------------------+
    | darey_io           |
    | information_schema |
    +--------------------+
    2 rows in set (0.000 sec)
Create a table called devops_list in the database created 

    CREATE TABLE darey_io.devops_list (
	item_id INT AUTO_INCREMENT,
	content VARCHAR(255),
	PRIMARY KEY(item_id)
    );
Add a few lines to the list

    INSERT INTO darey_io.devops_list (content) VALUES ("Linux needed for devops");
    INSERT INTO darey_io.devops_list (content) VALUES ("Git needed for devops");
    INSERT INTO darey_io.devops_list (content) VALUES ("LAMP needed for devops");

Check that the lines have been successfully inputed

    SELECT * FROM darey_io.devops_list;
To output below will be shown 

    +---------+--------------------------+
    | item_id | content                  |
    +---------+--------------------------+
    |       1 | Linux needed for devops  |
    |       2 | Git needed for devops    |
    |       3 | LAMP needed for devops   |
    +---------+--------------------------+
    3 rows in set (0.000 sec)
Exit mysql shell.

In the /var/www/html we need to edit our index.php file

    sudo nano /var/www/html/index.php
Paste the content below 

    <?php
    $user = "darey";
    $password = "Ab123456789";
    $database = "darey_io";
    $table = "devops_list";
    
    try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "<h2>Devops</h2><ol>"; 
    foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
    }
    echo "</ol>";
    } catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
    }
Save and close the file
Check you web browser with your ubuntu instance ip address to see nginx serving php with the contents in the database displayed.
