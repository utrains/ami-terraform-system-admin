# Server creation steps:

## 1- Set up the server using terraform and aws :
- Create a folder nammed __utrains-project__
- Enter in the folder then create the file nammed __main.tf__, using __Visual Studio Code__
- copy the content of the main.tf file form this page to your own file
- make the terraform command to set-up the server


```
cd ~
mkdir utrains-project
cd utrains-project
mkdir project1
cd project1
code main.tf
paste below code into the main.tf file and then save.
terraform init
terraform plan
terraform apply -auto-approve
```
## main.tf file

```
# configured aws provider with proper credentials
provider "aws" {
  region    = "us-east-1"
  profile   = "default"
}

# create security group for the ec2 instance
resource "aws_security_group" "utrais_security_gp" {
  name        = "ec2 utrains security group"
  description = "allow access on ports 80 and 22 for httpd and ssh"

  # allow access on port 80 for Apache Server
  ingress {
    description      = "httpd access port"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }
  # allow access on port 22 ssh connection
  ingress {
    description      = "ssh access"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }
  egress {
    from_port        = 0
    to_port          = 0
    protocol         = -1
    cidr_blocks      = ["0.0.0.0/0"]
  }
  tags   = {
    Name = "utrains server security group"
  }
}

# Generates a secure private key and encodes it as PEM
resource "tls_private_key" "instance_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}
# Create the Key Pair
resource "aws_key_pair" "instance_key" {
  key_name   = "instance_key_pair"  
  public_key = tls_private_key.instance_key.public_key_openssh
}
# Save file
resource "local_file" "ssh_key" {
  filename = "${aws_key_pair.instance_key.key_name}.pem"
  content  = tls_private_key.instance_key.private_key_pem
}

# launch the ec2 instance and install jenkis
resource "aws_instance" "ec2_instance" {
  # ami                    = "ami-0afd7c7d3a00702d3"
  ami                    = "ami-0cbdb0332336f1f92"
  instance_type          = "t2.medium"
  vpc_security_group_ids = [aws_security_group.utrais_security_gp.id]
  key_name               = aws_key_pair.instance_key.key_name
  //user_data            = file("installed_script.sh")

  tags = {
    Name = "utrains Server and ssh security group"
  }
}
# an empty resource block
resource "null_resource" "name" {
  # ssh into the ec2 instance 
  connection {
    type        = "ssh"
    user        = "ec2-user"
    private_key = file(local_file.ssh_key.filename)
    host        = aws_instance.ec2_instance.public_ip
  }
  # wait for ec2 to be created
  depends_on = [aws_instance.ec2_instance]
}
# print the url of the server
output "ssh_connection_command" {
  value     = join ("", ["ssh -i instance_key_pair.pem ec2-user@", aws_instance.ec2_instance.public_dns])
}
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

