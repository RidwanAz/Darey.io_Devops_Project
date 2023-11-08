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

3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance. See [Project 3]() to learn more about clonning git repositories.






