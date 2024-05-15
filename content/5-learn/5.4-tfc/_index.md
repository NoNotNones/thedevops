---
title : "Terraform Cloud"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---


#### Introduction

Terraform is a DevOps tool for declarative infrastructure—infrastructure as code. It simplifies and accelerates the configuration of cloud-based environments

- Source:
  - [Learn](https://www.linkedin.com/learning/learning-terraform-15575129/learn-terraform-for-your-cloud-infrastructure?resume=false&u=103729754)
  - [Github Repo](https://github.com/LinkedInLearning/learning-terraform-3087701)

#### First Setup

1. Fork Code repo:
- Repository name: **learning-terraform-3087701**
- Copy the **main** branch only

2. AWS
- IAM user: **tf-cloud**
  - Attach policies: **AdministratorAccess**
  - Security credentials:
    - Access key (**Third-party service**)

3. Terraform Cloud
- Website: https://app.terraform.io/app
  - Organizations: **tuanta_learn**
  - Choose your workflow: 
    - Version Control Workflow
      - Connect to a version control provider:  **Github.com**
      - Authorize cloud
      - Choose a repository: **learning-terraform-3087701**
        - Workspace Name: **learning-terraform**
      - Continue workspace overview ->

![54](/thedevops/images/5-learn/5.4/1.png?featherlight=false&width=50pc)

- Configure Variables
  - Category: Environment variable
    - Key :
      - AWS_ACCESS_KEY_ID   Value (sensitve)
      - AWS_SECRET_ACCESS_KEY Value (sentive)
![54](/thedevops/images/5-learn/5.4/2.png?featherlight=false&width=50pc)

#### Terraform Action
1. Terraform plan

- instance_type = "**t2.micro**"
- region  = "**ap-southeast-1**"

{{%expand "provider.tf" %}}
```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region  = "us-west-2"
}
```
{{% /expand%}}

{{%expand "variables.tf" %}}
```sh
#variable "instance_type" {
#  description = "Type of EC2 instance to provision"
#  default     = "t3.nano"
#}
```
{{% /expand%}}

{{%expand "outputs.tf" %}}
```sh
#output "instance_ami" {
#  value = aws_instance.web.ami
#}

#output "instance_arn" {
#  value = aws_instance.web.arn
#}
```
{{% /expand%}}

{{%expand "main.tf" %}}
```sh
data "aws_ami" "app_ami" {
  most_recent = true

  filter {
    name   = "name"
    values = ["bitnami-tomcat-*-x86_64-hvm-ebs-nami"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  owners = ["979382823631"] # Bitnami
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.app_ami.id
  instance_type = "t2.micro"

  tags = {
    Name = "HelloWorld"
  }
}
```
{{% /expand%}}

**learning-terraform** / Overview

- Start new plan
![54](/thedevops/images/5-learn/5.4/3.png?featherlight=false&width=50pc)
![54](/thedevops/images/5-learn/5.4/3-1.png?featherlight=false&width=50pc)

2. Terraform run

- Confirm & Apply
![54](/thedevops/images/5-learn/5.4/4.png?featherlight=false&width=50pc)
![54](/thedevops/images/5-learn/5.4/5.png?featherlight=false&width=50pc)

- Fix Issue : **No default VPC for this user**

Configure file:

AWS EC2 Key pair: **tf-cloud-keypair**

{{%expand "provider.tf" %}}
```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}

provider "aws" {
  region  = "us-west-2"
}
```
{{% /expand%}}
{{%expand "terraform.tfvars" %}}
```sh
vpc_cidr_block = "10.0.0.0/16"
subnet_cidr_block = "10.0.10.0/24"
avail_zone = "ap-southeast-1b"
env_prefix = "dev"
my_ip = "14.191.230.64/32"
instance_type = "t2.micro"
ami_id         = "ami-06c4be2792f419b7b"
```
{{% /expand%}}
{{%expand "main.tf" %}}
```sh
# Variables
variable vpc_cidr_block {}
variable subnet_cidr_block {}
variable avail_zone {}
variable env_prefix {}
variable instance_type {}
variable ami_id {}

# VPC & Subnet
resource "aws_vpc" "myapp-vpc" {
  cidr_block = var.vpc_cidr_block
  tags = {
      Name = "${var.env_prefix}-vpc"
  }
}

resource "aws_subnet" "myapp-subnet-1" {
  vpc_id = aws_vpc.myapp-vpc.id
  cidr_block = var.subnet_cidr_block
  availability_zone = var.avail_zone
  tags = {
      Name = "${var.env_prefix}-subnet-1"
  }
}

# Route table
resource "aws_route_table" "myapp-route-table" {
   vpc_id = aws_vpc.myapp-vpc.id

   route {
     cidr_block = "0.0.0.0/0"
     gateway_id = aws_internet_gateway.myapp-igw.id
   }
   tags = {
     Name = "${var.env_prefix}-rtb"
   }
 }

 # Internet gateway
 resource "aws_internet_gateway" "myapp-igw" {
        vpc_id = aws_vpc.myapp-vpc.id
    
    tags = {
     Name = "${var.env_prefix}-igw"
   }
}

# Associate subnet with Route Table
resource "aws_route_table_association" "a-rtb-subnet" {
  subnet_id      = aws_subnet.myapp-subnet-1.id
  route_table_id = aws_route_table.myapp-route-table.id
}

# Security Group
variable my_ip {}

resource "aws_security_group" "myapp-sg" {
  name   = "myapp-sg"
  vpc_id = aws_vpc.myapp-vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [var.my_ip]
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

### EC2 congifure
resource "aws_instance" "myapp-server" {
  ami                         = var.ami_id
  instance_type               = var.instance_type

  key_name                    = "tf-cloud-keypair"
  subnet_id                   = aws_subnet.myapp-subnet-1.id
  vpc_security_group_ids      = [aws_security_group.myapp-sg.id]
  availability_zone           = var.avail_zone
  associate_public_ip_address = true

  tags    = {
    Name = "${var.env_prefix}-server"
  }
}
```
{{% /expand%}}

Run Terraform plan
![54](/thedevops/images/5-learn/5.4/6.png?featherlight=false&width=50pc)

Run Terrafrom apply

![54](/thedevops/images/5-learn/5.4/7.png?featherlight=false&width=50pc)

AWS Console Check

![54](/thedevops/images/5-learn/5.4/8.png?featherlight=false&width=50pc)