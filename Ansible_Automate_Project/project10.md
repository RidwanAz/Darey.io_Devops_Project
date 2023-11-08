## Ansible_Automate_Project

## Implementing Ansible For Automation

### Step 1 Install and Configure Ansible on EC2 Instance
- Launch an instane and name it Jenkins-Ansible.
- Create a new repository call ansible-config-mgt. This repository will be connected to jenkins pipeline and also store ansible files
- Install Ansible

      sudo apt update
      sudo apt install ansible
  ***Confirm Ansible has been successfully installed***

      ansible --version
### Configure Jenkins Build Job To Archive Your Repository Content Every Time It Is Changed
Jenkin is a pipeline for continuous integration. To create a build job, we need to have jenkins installed and configured first.
- On the same EC2 instance ansible was installed, we need to install jenkins
*** Update package repositories

      sudo apt update
***Install JDK***

    sudo apt install default-jdk-headless
***Install Jenkins***

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
    sudo apt update
    sudo apt-get install jenkins
***Check if jenkins has been installed, and it is up and running***

    sudo systemctl status jenkins

***On our Jenkins port, open edit inbound rules and open port 8080***
![inbound]()

***Set up Jenkins***

i. Input your Jenkins-Ansible Instance ip address on your web browser i.e. http://public_ip_address:8080


ii. On your Jenkins-Ansible instance, check  "/var/lib/jenkins/secrets/initialAdminPassword" to know your password.


iii. Installed suggested plugins


iv. Create a user account and log in to jenkins

***Confiure Jenkins To Receive Source Code From ansible-cofig-mgt***


i. Allow webhook in our github repository. In ansible-config-mgt repository, navigate to **settings>webhooks** and past the url of jenkins.


ii. Create a freestyle project in your jenkins web account and name it "ansible_automation"


iii. Connect jenkins to ansible-config-mgt repository by pasting the repository url in the area selected below


iv. Save configuration and run "build now" to connect jenkins to our repository


v. Click "Configure" your job/project and add these two configurations

Configure triggering the job from GitHub webhook:
![webhook]()

Configure "Post-build Actions" to archive all the files.

click on add post-build actions and click "Archive the Artifact" then save

Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.

You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.


### Step 2:  Prepare your development environment using Visual Studio Code
1. First part of 'DevOps' is 'Dev', which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable - you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs - Visual Studio Code (VSC), you can get it here.

2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.

3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance. See [Project 1](https://github.com/RidwanAz/Darey.io_Devops_Project/blob/0ce5355a44b1709e01334537053a23fd480da176/Git_project/Git.md) step V to learn more about clonning git repositories.

### Step 3 - Begin Ansible Development
i. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.

ii. Checkout the newly created feature branch to your local machine and start building your code and directory structure

iii. Create a directory and name it playbooks - it will be used to store all your playbook files.

iv Create a directory and name it inventory - it will be used to keep your hosts organised.

v. Within the playbooks folder, create your first playbook, and name it common.yml

vi. Within the inventory folder, create an inventory file () for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively. These inventory files use .ini languages style to configure Ansible hosts.


### Setup Ansible Inventory 

In Ansible, an inventory file is a simple text file that defines the hosts and groups of hosts that Ansible can manage. It serves as a source of truth for Ansible to know which hosts it should target when running tasks and playbooks. The inventory file is a fundamental concept in Ansible and is used to organize and manage your infrastructure.

We need setup our ssh keys for ansible so it can have access to our host (remote servers) when running playbooks for our hosts.

This can be done using an SSH agent. An SSH agent allows you to securely store and manage your SSH private keys and provides a convenient way for ansible to authenticate with remote hosts without repeatedly entering your SSH key.


    eval `ssh-agent -s`
    ssh-add <path-to-private-key>
Confirm the key has been added with the command below, you should see the name of your key

    ssh-add -l 


Since we have our SSH key for our host stored with SSH agent so ansible can use this, ansible also need to know the host to manage. We can do this with an inventory file

i. Open the inventory/dev.yml file we created earlier
    
    sudo nano /inventory/dev.yml
ii. Paste the below information

    [nfs]
    ansible_host=<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user

    [webservers]
    ansible_host=<Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user
    ansible_host=<Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user

    [db]
    ansible_host=<Database-Private-IP-Address> ansible_ssh_user=ec2-user 

    [lb]
    ansible_host=<Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu


















