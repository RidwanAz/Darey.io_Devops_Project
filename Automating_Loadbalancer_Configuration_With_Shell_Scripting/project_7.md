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

# Project Highlight

# Deploying And Configuring Webservers and Load Balancer

#### Step 1: Launch 3 EC2 Instances
We will provision 3 EC2 instance, two will of these will be our webserver and one will be a load balaner

i. Launch 2 EC2 instances and name each "webserver_1" and "webserver_2"

ii. Launch another EC2 instance and name it "load balancer"

#### Step 2: Open New Security Group For Both Webservers and load balancer

i. For the webservers and load balancer, go to the security groups

ii Edit inbound rules on open port 8000 for our both webserver_1 and webserver_2 and port 80 for our load balancer

iii. Allow traffic from anywhere on the open ports


#### Step 3: Automating Webservers Configurartion With Shell Script

Paste the shell script below in a file on both webservers to configure each webservers
