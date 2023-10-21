# Implememting Load Balancers With Nginx

# Introduction
In some of our previous projects, we have worked with nginx. Nginx is a web serveer capable of load balancing and reverse proxy. In this project we will dive deeper into the usage of nginx for load balaning.

#### Concept of Load Balancing
Have you ever wondered how big tech companies like facebook, netflix and amazon always have their services available to their high number of users. That is because they have load balancers that distribute traffic aross multiple servers
Load balancing is the process of distributing a set of tasks over a set of resources, thereby making the overall process of those tasks more efficient.

In this project, we will use nginx to distribute traffic across 2 different servers.


![load balancer](image/load-balancer.png)

The image above show multiple client making request to a load balancer. The load balancer, make a request to a web server and gets a response then give the response back to the client. If the web server not available due to certain reasons which can either be that the server is down or cannot or cannot handle more requests, the load balancer will direct the client response to another server and give response back to the client.

#### Importance of a Load Balancer
From what we understand about the concept of load balancing, A load balancer is very important in modern IT and web services because it helps make sure applications work well and are always available. It does this by spreading network traffic across several servers. By making sure that no server gets too busy, it makes sure that everything works well and is reliable, fast, and can handle problems. This means that web application and web sites will still work and be fast even if lots of people are using them or if a server stops working. This helps make things better for users and makes online services stronger for industries like e-commerce, content delivery, and cloud computing.

#### Target Audience

DevOps Engineers: People who automate the management of servers and help applications run smoothly and efficiently, even when there are problems.

System administrators are people who want to improve the availability and speed of websites, especially when there are a lot of visitors or when the website is really important.

Web Developers: People who create and improve websites want to make sure that the websites work well and consistently, especially when there are changes in the number of people visiting the website.

Network administrators are in charge of setting up and looking after the network infrastructure and services.


#### Project Prerequisites

- An AWS account
- Knowledge of Linux
- Basic understanding of Web Servers such as apache and nginx
- Familiarity with linux text editors such as vim, emacs and nano

# Project Hghlight

# Implementation of Load Balancers With Nginx
#### Step 1: Provisioning EC2 Instances

We will provision 3 EC2 instance, two will of these will be our webserver and one will be a load balaner

i. Launch 2 EC2 instances and name each "webserver_1" and "webserver_2"

ii. Launch another EC2 instance and name it "load balancer"

#### Step 2: Open New Security Group For Both Webservers and load balancer

i. For the webservers and load balancer, go to the security groups

ii Edit inbound rules on open port 8000 for our both webserver_1 and webserver_2 and port 80 for our load balancer

iii. Allow traffic from anywhere on the open ports

#### Step 3: Install Apache Webserver

i. Open 2 termianls and ssh into webserver_1 and webserver_2

  ***For windows, open command prompt or git bash and ssh into the webservers***

ii. Update and upgrade package lists 

    sudo apt update -y && sudo apt upgrade -y

iii. Install Apache 

    sudo apt install apache2 -y

iv. Confirm Apache has been successfully installed

    sudo systemctl status apache2

#### Step 4: Configuring Apache to Port 8000

By default, apache listen on port 80. Since our load balancer will also be listening on port 80, we  need to make our webservers listen on port 8000

i. Grant users permission to read, write and execute the file ports.conf file

    sudo chmod u+rwx ports.conf
ii. Edit port.conf file 

    sudo vi /etc/apache2/ports.conf
iii. Add a new listen directive

![listen](images/listen.png)

iv. Add a new virtualhost statement since a new listen directive has been added

    sudo vi /etc/apache2/sites-available/000-default.conf

![virtualhost](images/virtualhost.png)

v. Reload Apache

    sudpo systemctl reload apache2

** Note this step should be done for both webservers

#### Step 5: Configuring Apache to show names of both webservers

In the previous step, we set up a new listen directive and virtualhost statement. In our "000-default.conf", the document root is located at /var/www/html. We need to change the content of the file in our document root to show the name of our webserver. 

i. Edit index.html in /var/www/html

    sudo nano /var/www/html/index.html
ii. Delete the content in the file and paste the content below

      <!DOCTYPE html>
        <html>
        <head>
            <title>Apache EC2 Instance</title>
        </head>
        <body>
            <h1>Load Balancer</h1>
            <p>Webserver 1 & 2</p>
        </body>
        </html>

iii. Save and close the file

iv. Restart Apache

    sudo systemctl restart apache2
***Note:*** This step should be done for both webserver1 and webserver 2

#### Step 6: Install and Configure Nginx As A Load Balancer For Both WebServers 

In the step, we will configure nginx as a load balancer for webserver 1 and 2

On our load balancer instance;

i. Update package lists and instal nginx

    sudo apt update -y && sudo apt install nginx -y

ii. Verify that Nginx is successfully installed 

    sudo systemctl status nginx

iii. Edit Nginx load balancer configuration file

    sudo nano /etc/nginx/conf.d/loadbalancer.conf


iv. Paste the configuration file below to configure nginx to act like a load balancer. A screenshot of an example config file is shown below: Make sure you edit the file and provide necessary information like your server IP address etc.

        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 127.0.0.1:8000; # public IP and port for webserser 1
            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
    
upstream backend_servers defines a group of backend servers. The server lines inside the upstream block list the addresses and ports of your backend servers. proxy_pass inside the location block sets up the load balancing, passing the requests to the backend servers. The proxy_set_header lines pass necessary headers to the backend servers to correctly handle the requests

v. Test if nginx configuration is correct

    sudo nginx -t

vi Restart nginx

    sudo systemctl restart nginx

vii. Paste load balancer public Ip address on your web browser to see the content of web server 1 and 2
