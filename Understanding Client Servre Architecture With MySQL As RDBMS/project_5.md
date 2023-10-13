# Understanding Client Server Architecture With MySQL As RDBMS

# Introduction
When we talk the word client, it mean to talk of a person using a particular service. Similarly in the digital world a client can be a  computer or mobile phone capable of receiving information through a medium which can be a web browser or a terminal from a server.
The word server means a medium or a person that serves something. In the real world, a Server is a remote computer which provides information or access to particular services.

So it can basically be said that a client requests and a server respond. 

The image below shows a simple client server architecture.

![cs](images/cs.png)

As we proceed is this project, we will understand more about a [client server architecture](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) and also implement a client server architecture with mysql as a relational database management system.



### Client Server Architecture With MySQL

MySQL is a relational database management system (RDBMS) developed by Oracle that is based on structured query language (SQL). Asw a devops engineer, a knowledge of a RDMS such as [MySQL](https://en.wikipedia.org/wiki/MySQL) is needed.

Now that we have an understanding of a client server architecture, a client server architecture with mysql means using mysql as a client as well as a server.

In some of our previous projects like LAMP and LEMP stack, we implemented, a client server architecture wherby our webserver is a client such that it sends a request to our database server to get a response (data). The architecture is shown inthe image below

![cs2](images/cs2.png)


### Why is a  Client Server Architecture With MySQL Important

A client-server system with MySQL is really important and needed to be engineered because it helps manage data efficiently and safely for a lot of different programs. By dividing the part that users see and use on their computers or devices from the part that manages the storage of information on a server, this design helps to make the system able to handle more users at the same time, as many users can use the database at once. The technology makes sure that data is safe, accurate, and stored in one place. It can be used in many industries like online shopping and healthcare. It is helpful when there is a lot of data that needs to be saved, accessed, and changed in an organized way. This helps to easily find, save, and change data at the same time.

### Target Audience

i. Software developers can learn about client-server architecture and MySQL to help them build applications that need a good way to store, find, and control data using a database server.

ii. Database administrators are people who can use this knowledge to make sure that MySQL databases work well and are safe and able to grow in a client-server setup.

iii. System administrators are people who are in charge of setting up, organizing, and taking care of server-client systems. They can make use of understanding the complexities of this structure to ensure smooth functioning and fix any problems.

iv. DevOps Engineers can use their knowledge of how clients and servers work together to automate the process of setting up, adjusting the size, and taking care of the software.


### Prerequisite

i. An AWS account

ii. Knowledege of [linux](http://linuxcommand.org/lc3_lts0020.php) and its commands.

iii SSH keys


In the project we will implement a client server architecture with mysql as a relational database management system

# Project Highlight


# Implementing MySQL as a Client Server Architecture 

#### Step 1: Launch 2 EC2 Instances on AWS
i. Each instance should be named

- Instance 1 - mysql server

- Instance 2 - mysql client

ii. Open 2 terminals and ssh into both "mysql server" "mysql client"
- For windows users, open command prompt, powershell or git bash
- For mac and linux users, open a terminal and ssh into the instance

#### Step 2: Updating and Upgrading Package Lists and Apt Repositories
On both mysql server and mysql client update and upgrade package lists

    sudo apt update -y && sudo apt upgrade -y

**Note:** The command above should be executed for mysql server and mysql client instances

#### Step 3: Installing MySQL Server Software

On mysql server instance install MySQL Server software.

    sudo apt install my-sql-server -y

To start mysql service

    sudo systemctl start mysql

Check is mysql is active and running

    sudo systemctl status mysql

#### Step 4: Installing MySQL Client Sofware 
On mysql client instance install MySQL Client software.

    sudo apt install mysql-client -y

#### Step 4: Allowing Access To For MySQL Server for MySQl Client

By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses.
Or, you can add them to the same subnets.

Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

#### Step 5: Create A New User and Database On MySQL Server 

In order for mysql client to be able to send request to mysql server, we need to create a new user for mysql client and a database.

L
For MySQL secure installation use the following,
secure install

After the installation you might need to create a password for root user
Alter User

On MySQL server create a user and a database
Create user

create database

Grant privileges
GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;

Flush Privileges
FLUSH PRIVILEGES

Exit MySQL and restart the mySQL service using
sudo systemctl restart mysql

You might need to configure MySQL server to allow connections from remote hosts.
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:
Configure MySQL server

From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.
Check that you have successfully connected to a remote MySQL server and can perform SQL queries:

Client connection to mysql

Show database

Show databases;

show database

If you see an output similar to the below image, then you have successfully completed this project – you have deloyed a fully functional MySQL Client-Server set up.

