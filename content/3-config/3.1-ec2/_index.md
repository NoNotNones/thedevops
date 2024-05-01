---
title : "IaC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

In this Workshop we will create an EC2 instances with the information bellow
#### Overview AWS EC2
-   Instances name: Web-Server
-   VPC: 10.0.0.0/16
-   Subnets: 10.0.1.0/24
-   Region: Singapore (ap-southeast-1)
-   Available zone: ap-southeast-1b
-   Instance type: t2.micro
-   Amazon Machine Images: Amazon Linux 2 AMI
-   Key pair: **tf-cli-keypair**
-   Security setting: 
    -   Only allow my ip connect SSH to EC2 instance
    -   Allow all access from port 8080 to EC2 instance

#### Terraform configuration

Security credential variables:  **variables.tf**

```dockercompose
variable "access_key" {
  type        = string
  sensitive   = true
}

variable "secret_key" {
  type        = string
  sensitive   = true
}

variable "region" {
  type        = string
  default     = "ap-southeast-1"
}
```
Instances configurations :**main.tf**

```main
variable vpc_cidr_block {}
variable subnet_1_cidr_block {}
variable avail_zone {}
variable env_prefix {}
variable instance_type {}
variable my_ip {}
variable ami_id {}

resource "aws_vpc" "myapp-vpc" {
  cidr_block = var.vpc_cidr_block
  tags = {
      Name = "${var.env_prefix}-vpc"
  }
}

resource "aws_subnet" "myapp-subnet-1" {
  vpc_id = aws_vpc.myapp-vpc.id
  cidr_block = var.subnet_1_cidr_block
  availability_zone = var.avail_zone
  tags = {
      Name = "${var.env_prefix}-subnet-1"
  }
}

resource "aws_security_group" "myapp-sg" {
  name   = "myapp-sg"
  vpc_id = aws_vpc.myapp-vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port       = 0
    to_port         = 0
    protocol        = "-1"
    cidr_blocks     = ["0.0.0.0/0"]
    prefix_list_ids = []
  }

  tags = {
    Name = "${var.env_prefix}-sg"
  }
}

resource "aws_internet_gateway" "myapp-igw" {
	vpc_id = aws_vpc.myapp-vpc.id
    
    tags = {
     Name = "${var.env_prefix}-internet-gateway"
   }
}

resource "aws_route_table" "myapp-route-table" {
   vpc_id = aws_vpc.myapp-vpc.id

   route {
     cidr_block = "0.0.0.0/0"
     gateway_id = aws_internet_gateway.myapp-igw.id
   }

   # default route, mapping VPC CIDR block to "local", created implicitly and cannot be specified.

   tags = {
     Name = "${var.env_prefix}-route-table"
   }
 }

# Associate subnet with Route Table
resource "aws_route_table_association" "a-rtb-subnet" {
  subnet_id      = aws_subnet.myapp-subnet-1.id
  route_table_id = aws_route_table.myapp-route-table.id
}

output "server-ip" {
    value = aws_instance.myapp-server.public_ip
}

resource "aws_instance" "myapp-server" {
  ami                         = var.ami_id
  instance_type               = var.instance_type
  key_name                    = "tf-cli-keypair"
  associate_public_ip_address = true
  subnet_id                   = aws_subnet.myapp-subnet-1.id
  vpc_security_group_ids      = [aws_security_group.myapp-sg.id]
  availability_zone			  = var.avail_zone

  tags = {
    Name = "${var.env_prefix}-server"
  }
}

```
Terraform provider **AWS** :  **terraform.tfvars**

```tfvars
# Network and Instance variables
vpc_cidr_block = "10.0.0.0/16"
subnet_1_cidr_block = "10.0.1.0/24"
avail_zone = "ap-southeast-1b"
env_prefix = "web"
my_ip = "<myip>/32"
ami_id = "ami-04f73ca9a4310089f"
```
   
### Installation
Terraform plan: 
```dockercompose
 docker-compose run –rm terraform plan
```
![31](/cicd-ws/images/3-config/3.1-ec2/1.png)

Terraform apply: 
```dockercompose
 docker-compose run --rm terraform apply --auto-approve
```
![23](/cicd-ws/images/3-config/3.1-ec2/2.png)

-   AWS Instance checking:
![23](/cicd-ws/images/3-config/3.1-ec2/4.png?featherlight=false&width=90pc)

Add Keypair permission:
```dockercompose
chmod 400 tf-cli-keypair.pem 
```
SSH to EC2 Instances:

```dockercompose
 ssh -i tf-cli-keypair.pem ec2-user@13.250.64.49
```
-   AWS Instance checking:
![23](/cicd-ws/images/3-config/3.1-ec2/3.png?featherlight=false&width=90pc)