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

ii. 



