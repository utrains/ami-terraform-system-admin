# Server creation steps:

## 1- Set up the server using terraform and aws :
- Create a folder
- Enter in the folder 
- Clone this repository in your local machine
- make the terraform command to set-up the server
```

cd ~
mkdir utrains-project
cd utrains-project
git clone https://github.com/utrains/ami-terraform-system-admin.git
cd Project1
terraform init
terraform apply -auto-approve
```
## 2- Login to the server:
- Copy the output command for terraform to login in the server: something like this 
![alt text](../media/login_img.PNG)


# project_linux_administrator  


#### 1. Consider you are a linux Engineer. During the release to production, a deployment is failing, with the following error : 
#### /opt/deployment/uat/deploy.cfg: No such file or directory, Deployment failed!! 
#### So the developers code is trying to access the deploy.cfg file to no avail.
#### Check on the server, if the path that the developer has put in his code is correct (provide the correct path to the developers if /opt/deployment/uat/deploy.cfg is not correct). 

#### 2.  There is a file on the system called success.txt, run a command to display its content.

#### 3. Add this sentence to the file success.txt  "I am staying laser focus and wont let anything distract me at all"

#### 4.  What group does the success.txt file belongs to ?  

#### 5.  What is the permission on success.txt file for others people on the system

#### 6.  We want the file success.txt to be owned by the user u5bt , how can we do that?

#### 7.  The user u5bt's full name is "John Mary" please modify their account to reflect this.

#### 8.  Change the permission on the file success.txt to reflect the access below :
##### - owner: full permission
##### - group: read and write
##### - others: no permission what so ever

#### 9.  Change the password for the user u5bt to redhat

#### 10.  Create a user sarah with no access to an interactive shell

#### 11.  Create a group called network

#### 12.  Create a user called henry with network as subgroup

#### 13.  Herman is a new contractor in the team and is complaining that he can not login to this server. 
#### please investigate what can be the issue and solve it so he can login to the server

#### 14.  Go ahead and force Herman to change his password on his next login

#### 15.  A deployment to this server is failing and apparently it is due to selinux been set to enforcing mode. You are tasked to disable it. Below are the steps to do that. go ahead and do it.
######    vi /etc/selinux/config
######    change
######    SELINUX=enforcing
######    to
######    SELINUX=permissive
######    save and quit
######    NB:( you can read more about selinux feature later on this link https://www.redhat.com/en/topics/linux/what-is-selinux ) 
#### 16.  This server will be used as a webserver. Please go ahead and configure apache on it.
#### It should display below message on the browser
## "I feel like I am sitting in the office working for real and making the big $$$$"

