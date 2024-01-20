# Project Name : Temporary Notes

# Tools Used- 
Git,
Github,
Docker,
Jenkins

# About Project
This is simple project for beginners to undertands basic cicd pipeline.
First we push the code on github using git and after that for automation of project we use the jenkins for cicd pipeline.

We fetch the code on server again using jenkins in jenkins project path after that we create a docker image by using docker file and build image and we run that image. Now Our Temporay notes project is sucessfuly deployed. After that we again do the atomation by using the jenkins execute shell option to build and run the image.

# Step 1 : Install Jenkins
You can also install jenkins using this url - https://pkg.jenkins.io/debian/

sudo apt-get update

This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system (for the Weekly Release Line):

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian/jenkins.io-2023.key
	
Then add a Jenkins apt repository entry: 

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
	
Update your local package index, then finally install Jenkins:
 
  sudo apt-get update
  sudo apt-get install fontconfig openjdk-17-jre
  sudo apt-get install jenkins
	
# Step 2:Go in security group of your ec2 instance and Edit Inbound rules of your ec2 instance add port number 8080 for jenkins
In browser search your address with 8080 port example - http://13.232.62.44:8080/

You will see the jenkins dashboard.

# Step 3:Sign up jenkins
use below cmd to get the jenkins password

cat /var/lib/jenkins/secrets/initialAdminPassword

Select Install suggested plugin in jenkins


# Step 4:Install docker
sudo apt-get update

sudo apt install docker.io

docker  --version

# Step 5: Git clone
git clone https://github.com/RushikeshVikhe/MyProject.git

run below git cmds in your project files location:

git init

git status

git log ... if you want to see what you have added, press ctrl+z to go back

git commit -m "This is My First Commit"

git remote add origin <your github repo url>

git push -u origin master


others cmds:
git restore filename  ---we we delete file then it is used to restore

git remote -v         ---want to see what that already existing origin remote is

git remote set-url origin <URL>  ---update the existing remote


# create private and public keys:
ssh-keygen

go to github settings - >SSH and GPG keys->New SSH Key->Paste ssh public key->add ssh key

# Step 6:Create new Job
Select free style project

Give discription of your project

Select github project option and paste the github url of project(means browser url of repository)

Source Code Management->select git->paste the github url of repository where you pushed in the code ->

Add credentials ->jenkins->SSH username with private keyrings/jenkins-keyring

Domain-Global credentials (unrestricted)

Kind-SSH Username with private key

Scope-Global (Jenkins, nodes, items, all child items, etc)

ID-github-jenkins

Description-This is for Github and jenkins integration

Username-ubuntu

Private Key-Enter directly->paste private key

Build Triggers->GitHub hook trigger for GITScm polling(checkbox)->Save


# Step 7 :Build Now the job
After successful build you will see the project path of project in output console example - /var/lib/jenkins/workspace/project_name

# Step 8:Go in security group of your ec2 instance and Edit Inbound rules of your ec2 instance add port number 8000 for application

# Step 9 :Run these commands now :
sudo apt install nodejs

sudo apt install npm

npm install

node app.js.....by running this cmd your webpage is working to see-> search ipaddress with 8000 port example->http://15.206.173.52:8000/

or Run by docker compose
test

Press ctrl+z to stop


# Step 10:Now to automate this process
Write docker file -

FROM node:12.2.0-alpine

WORKDIR app

COPY . .

RUN npm install

RUN npm run test

EXPOSE 8000

CMD ["node","app.js"]


# Steps 11: Add inbound rules http,https,ssh

sudo usermod -a -G docker $USER

sudo reboot or reboot your ec2 instace

docker build . -t <img_name>  ....build image by using this cmd

doker images

docker run -d --name <container_name> -p 8000:8000 <img_name>

# step 12: Now to automate this process using jenkins
docker kill <runing container_name>

docker image rm <img-name>


# Step 13: Follow below commands
sudo usermod -a -G docker jenkins

sudo systemctl restart jenkins

Select job ->go to configure jobs->scroll down select Build Steps->Execute shell->paste cmds

docker build . -t <img-name>

docker run -d --name <container_name> -p 8000:8000 <img-name>

Select Build Now

# Step 13:for Webhooks
in jenkins go to manage jenkins->available Plugins-> search github integration and install->restart jenkins

Go in Repository Settings->webhooks->add webhook->http://15.206.173.52:8080/github-webhook/     .....(jenkins_url/github-webhook/)

Now you can make code in github code and commmit, jobs runs automatically





