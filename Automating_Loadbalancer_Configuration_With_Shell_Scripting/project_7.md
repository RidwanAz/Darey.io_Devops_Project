# Automating Loadbalancer Configuration With Shell scripting

# Introduction
In some of the previous project, we implemented shell scripting for automation. Also in the last project, we manually implemented load balancer with nginx.

The project is all about automating the manual process of the last project.

### What is Automation ?

Automation is the process of using morden technology to eliminate manual process. These technologies can do many tasks automatically, from simple repititive task to more complicated tasks that involve making decisions.

A quote by Larry Wall  "A good engineer is a lazy one". This is because good engineers are always looking for was to automate tasks thereby making their work efficient.

As DevOps Engineers automation is at the heart of the work we do. Automation helps us speed the the deployment of services and reduce the chance of making errors in our day to day activity.

### Importance Of Automation

Automating Load Balancers with Nginx is very important in the world of technology and industry. It helps manage web traffic distribution smoothly and efficiently. In a situation whereby more and more people are using web applications and services, using Nginx to automatically balance the workload can help make it easier to deploy and scale these applications. This can result in better performance, more reliability, and less time where the application is not available. This automation helps save time and resources. It is very important for industries like online shopping, content delivery, and microservices. The ability to do this project well helps learners keep up with the needs of automtion and its  importance as a  devops engineer.

### Target Audience

- DevOps engineers aiming to enhance the scalability and reliability of their web applications through automated load balancing will find this project invaluable.
- System admins looking to optimize server performance, manage traffic efficiently, and ensure high availability for web services will benefit from mastering automated load balancing with Nginx.
- Software engineers interested in understanding how load balancing works and how it can be automated within their applications to improve user experience and performance.

### Project Prerequisite

- An AWS account
- Basic knowlwdge of Linux
- Previous Knowledge of [shell scripting](https://github.com/RidwanAz/Darey.io_Devops_Project/blob/a39ef56ea1c33036b75eb59bfc6bf2a56f250548/Shell_scripting/Shell.md)
- Knowledge of using text editors
- Basic knowledge of webservers

#### Project Goals
The primary goals and learning objectives of the "Automating Load Balancers with Nginx" project are to empower readers to automate the installation and configuration of Nginx for load balancing, thereby achieving improved web application performance, high availability, and efficient traffic distribution. By the end of this tutorial, learners should understand the importance of load balancing automation, grasp the fundamentals of Nginx configuration, and be equipped with the practical skills to automate load balancing in their web server environments, making them proficient in enhancing the reliability and scalability of web applications.

# Project Highlight

- Automating Load Balancer Configuration With Shell Scripting

- Introduction
  - What Is Automation
  - Importance Of Automating Load Balancer Configuration With Shell Scripting
  - Target Audience
  - Project Prerequisites
  - Project Goals

- Deploying And Configuring Webservers And Load Balancer
  - Launch 3 EC2 Instances
  - Open New Security Group For Both Webserver And Load Balancer
  - Automating Webservers Configuration With Shell Sript
  - Automating Load Balancer Configuration With Shell Script

- Real Life Scenarios of Load Balancing With Nginx
- Conclusion

# Deploying And Configuring Webservers and Load Balancer

#### Step 1: Launch 3 EC2 Instances
We will provision 3 EC2 instance, two will of these will be our webserver and one will be a load balaner

i. Launch 2 EC2 instances and name each "webserver_1" and "webserver_2"

ii. Launch another EC2 instance and name it "load balancer"

#### Step 2: Open New Security Group For Both Webservers and load balancer

i. For the webservers and load balancer, go to the security groups

ii Edit inbound rules on open port 8000 for our both webserver_1 and webserver_2 and port 80 for our load balancer

iii. Allow traffic from anywhere on the open ports

iv. ssh into the three instances


#### Step 3: Automating Webservers Configurartion With Shell Script

i. Paste the shell script below in a file on webserver 1 and 2 instances to configure each webservers

    #!/bin/bash

    ####################################################################################################################
    ##### This automates the installation and configuring of apache webserver to listen on port 8000
    ##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
    ######## ./install_configure_apache.sh 127.0.0.1
    ####################################################################################################################

    set -x # debug mode
    set -e # exit the script if there is an error
    set -o pipefail # exit the script when there is a pipe failure

    PUBLIC_IP=$1   # place the public ip address of each webserver

    [ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

    sudo apt update -y &&  sudo apt install apache2 -y

    sudo systemctl status apache2

    if [[ $? -eq 0 ]]; then
        sudo chmod 777 /etc/apache2/ports.conf
        echo "Listen 8000" >> /etc/apache2/ports.conf
        sudo chmod 777 -R /etc/apache2/

        sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

    fi
    sudo chmod 777 -R /var/www/
    echo "<!DOCTYPE html>
           <html>
           <head>
               <title>My EC2 Instance</title>
           </head>
           <body>
               <h1>Welcome to my EC2 instance</h1>
               <p>Public IP: "${PUBLIC_IP}"</p>
           </body>
           </html>" > /var/www/html/index.html

    sudo systemctl restart apache2

<table>
  <tr>
    <td><img src="image/install1.png" alt="Image 1"></td>
    <td><img src="image/install2.png" alt="Image 2"></td>
  </tr>
</table>

***Note:*** In  PUBLIC_IP=$1, place the public Ip address of each webserver

- Explanation of the shell script above

**Shebang:** The script starts with a shebang line #!/bin/bash, indicating that it should be executed using the Bash shell.

**Comments:** The script contains comments that provide explanations and usage instructions.

**Debug Mode:** set -x enables debug mode, which means that each command is printed to the console before it is executed. This is useful for troubleshooting and understanding the script's execution flow.

**Exit on Error:** set -e causes the script to exit immediately if any command returns a non-zero exit status, indicating an error.

**Exit on Pipe Failure:** set -o pipefail ensures that the script exits if any command within a pipeline fails.

**User Input:** The script expects the public IP address of the EC2 instance as a command-line argument. It assigns this value to the PUBLIC_IP variable.

**Argument Check:** The script checks if the PUBLIC_IP variable is empty and prints an error message if it is, along with usage instructions. If the IP is missing, the script exits with an error code.

**Update and Install Apache:** The script updates the package list with sudo apt update -y and installs the Apache web server with sudo apt install apache2 -y.

**Check Apache Status:** It checks the status of the Apache service using sudo systemctl status apache2. If the service is running successfully (return code 0), the script proceeds with the configuration.

**Configuration Changes:** The script makes the following configuration changes to Apache:


It grants full read and write permissions to /etc/apache2/ports.conf to allow editing the configuration file.

It appends Listen 8000 to the end of /etc/apache2/ports.conf, which instructs Apache to listen on port 8000.

It grants full read and write permissions recursively to the /etc/apache2/ directory.

It replaces the <VirtualHost *:80> line with <VirtualHost *:8000> in the Apache default site configuration file (/etc/apache2/sites-available/000-default.conf), ensuring Apache listens on port 8000.

**Set Up Web Page:** The script grants full read and write permissions recursively to the /var/www/ directory and creates an HTML file (index.html) in the default web directory (/var/www/html). This HTML file displays a simple web page with the instance's public IP address.

**Restart Apache:** Finally, the script restarts the Apache service with sudo systemctl restart apache2 to apply the configuration changes.

ii. Save and run the script. Check [here](https://github.com/RidwanAz/Darey.io_Devops_Project/blob/a39ef56ea1c33036b75eb59bfc6bf2a56f250548/Shell_scripting/Shell.md) to see syntax of running shell script
<table>
  <tr>
    <td><img src="image/bash1.png" alt="Image 1"></td>
    <td><img src="image/bash2.png" alt="Image 2"></td>
  </tr>
</table>


<table>
  <tr>
    <td><img src="image/web01.png" alt="Image 1"></td>
    <td><img src="image/web02.png" alt="Image 2"></td>
  </tr>
</table>

#### Step 4: Automating Load Balancers Configurartion With Shell Script

On our load balancer instance;

i. Open a file and paste the shell script below

    #!/bin/bash

    ######################################################################################################################
    ##### This automates the configuration of Nginx to act as a load balancer
    ##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
    ##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
    ##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
    #####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
    ############################################################################################################# 

    PUBLIC_IP=$1 # input your load balacer public ip adress
    firstWebserver=$2 # input your webserver_1 public ip address and port e.g 127.0.0.1:8000
    secondWebserver=$3 # input your webserver_1 public ip address and port e.g 127.0.0.1:8000

    [ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

    [ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the    
    second argument to the script" && exit 1

    [ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the 
    third argument to the script" && exit 1

    set -x # debug mode
    set -e # exit the script if there is an error
    set -o pipefail # exit the script when there is a pipe failure


    sudo apt update -y && sudo apt install nginx -y
    sudo systemctl status nginx

    if [[ $? -eq 0 ]]; then
        sudo touch /etc/nginx/conf.d/loadbalancer.conf

        sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
        sudo chmod 777 -R /etc/nginx/

    
        echo " upstream backend_servers {

                # your are to replace the public IP and Port to that of your webservers
                server  "${firstWebserver}"; # public IP and port for webserser 1
                server "${secondWebserver}"; # public IP and port for webserver 2

                }

               server {
                listen 80;
                server_name "${PUBLIC_IP}";

                location / {
                    proxy_pass http://backend_servers;   
                }
        } " > /etc/nginx/conf.d/loadbalancer.conf
    fi

    sudo nginx -t

    sudo systemctl restart nginx

![install](image/install3.png)

ii. Save and close the file

iii. Run the script
![bash](image/bash3.png)
<table>
  <tr>
    <td><img src="image/lb01.png" alt="Image 1"></td>
    <td><img src="image/lb02.png" alt="Image 2"></td>
  </tr>
</table>

## Side Study
Configuration Management: Learn about of configuration management systems like Ansible, which enable you to automate load balancer setup and maintenance across your infrastructure.

# Real Life Scenarios With Load Balancers

- Managing the distribution of workload for a very busy online shopping website. Imagine you have a very popular online store that gets a lot of visitors, especially when there are sales happening. Load balancing with Nginx means that when people visit a website, their requests are spread out evenly across many different servers that work together. This stops one server from getting overwhelmed, which makes everything work better and leads to a better shopping experience for your customers. In this situation, you can set up Nginx to distribute the workload evenly using different methods like rotating through each server, selecting the server with the least connections, or assigning specific servers based on the IP address. This ensures that resources are used efficiently.

- Ensuring that a Content Delivery Network (CDN) is always available. Imagine you are in charge of overseeing a Content Delivery Network (CDN) that delivers multimedia content like videos and images to people everywhere in the world. Nginx's load balancing feature helps spread out content requests among many servers located in different places. Nginx's health checks can find problems with servers and redirect visitors to servers that are working properly. This helps make sure that content can still be accessed when a server isn't working. This example shows how Nginx helps make sure a distributed system can stay operational and handle errors well.

- Scalability for a Microservices Architecture: The ability to handle increasing amounts of work or traffic in a microservices architecture. In a modern application setup with microservices, you have many separate services running on different servers. Load balancing is important for evenly spreading incoming requests among these small services. Nginx can be like a traffic controller for an API, directing client requests to the correct microservices according to certain rules and traffic patterns. This example shows how Nginx can make a microservices-based system more scalable, manageable, and reliable.


# Conclusion
The project give learners important knowledge about automation. It helps you learn important skills to improve the performance, stability, and ability to handle more users of your web application by automating the load balancing process. This project will help you build strong, effective, and reliable web applications, and is a useful tool for DevOps engineers. Your adventure in this world has just started, and there are countless opportunities to use this information in real-life situations to make your web services better and more accessible.
