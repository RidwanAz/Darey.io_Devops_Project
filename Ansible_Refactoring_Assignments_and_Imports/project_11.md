## Ansible Refactoring, Assignments and Imports


## Refactor ansible code by importing other playbooks into site.yml
### Step 1: Jenkins job enhancement
In the previous project, we create a jenkins build job, let us make some changes to our Jenkins job. Now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job - we will require Copy Artifact plugin.

i. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact - we will store there all artifacts after each build.
sudo mkdir /home/ubuntu/ansible-config-artifact

ii. Change permissions to this directory, so Jenkins could save files there - chmod -R 0777 /home/ubuntu/ansible-config-artifact

iii. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins.

iv. Create a new Freestyle project as it was done in the previous project and name it save_artifacts. Make sure it is connected to ansible-config-mgt repository.

v. This project will be triggered by completion of your existing ansible project. Configure it accordingly

vi. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

vii. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).


### Step 2: Refactor Ansible Code By Importing Other Playbooks Into site.yml

i. Within playbooks folder, create a new file and name it site.yml - This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

ii. Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

iii. Move common.yml file into the newly created static-assignments folder.

iv. Inside site.yml file, import common.yml playbook.








