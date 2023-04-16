#Project3   version: NA  


## 1- Set up the server using terraform and aws :
- Create a folder nammed __utrains-project__
- Enter in the folder then create the file nammed __main.tf__, using __Visual Studio Code__
- copy the content of the main.tf file form this page to your own file
- make the terraform command to set-up the server 

```
cd ~
mkdir utrains-project
cd utrains-project
mkdir project3
cd project3
code main.tf
paste below code into the main.tf file and then save.
terraform init
terraform plan
terraform apply -auto-approve
```
## min.tf file

```
# configured aws provider with proper credentials
provider "aws" {
  region    = "us-east-1"
  profile   = "default"
}

# create security group for the ec2 instance
resource "aws_security_group" "utrains_sonar_security_gp" {
  name        = "ec2 sonar utrains security group"
  description = "allow access on ports 9000 and 22 for sonar and ssh"

  # allow access on port 9000 for Sonar Server
  ingress {
    description      = "httpd access port"
    from_port        = 9000
    to_port          = 9000
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
  ami                    = "ami-0afd7c7d3a00702d3"
  instance_type          = "t2.medium"
  vpc_security_group_ids = [aws_security_group.in._sonarid]
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


###  1- At work you are in the middleware team and as such, there is a request from the DevOps team to build a Sonarqube server. 
### The documentation on the process to follow is in the confluence page. follow that and install a Sonarqube server for the devops team.
### The deliverable will be the url of Sonarqube server, the username and password to access Sonarqube.
### Confluence url:  https://dataservicegroup.atlassian.net/wiki/spaces/RET/pages/1991246045/Sonarqube+installation+on+Centos-7
### username: unixtteam24@gmail.com
### password: school123

### 2- AT work, there are documents for installation of various applications stored in confluence, and during the meeting , the obersavation was that some of them need to be automated.
### a- Go ahead and automate the installation of Sonarqube, using a bash shell script. ( we should be able to copy the script to the server and execute it.)

### b- For better collaboration at work, we need to keep the scripts in github. Create a repo in github called middleware-scripts and push your scripts there. 