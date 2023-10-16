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

