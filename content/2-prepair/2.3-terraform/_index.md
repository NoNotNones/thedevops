---
title : "Terraform"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

Terraform is a open-source tool used to build, modify, and version control infratrucrure
#### Overview
- **Provider**(`provider.tf`):
        Enables Terrafrom to interact with cloud providers and other APIs
- **Terraform** (`versions.tf`):
        Sets version constaints for Terraform and optionally maps provides to a source address and version constaint
- **Variables** (`variable.tf`):
        Input variables define reusable values and work like function arguments in general-purpose programming languages
- **Resource** (`main.tf`):
         Resource blocks describe infrastructure objects like VPCs, subnets, route tables, and gateways
- **Data** :
         Data sources allow Terraform to ultilize information form resources that were defined outside of Terraform (or defined a different Terraform configuration)
- **Output**:
         Outputs return structured data form your configuration and work like return values in generaral-purpose programming languages 
- **Terraform.tfvars**:
        To set lots of variables, it is more convenient to specify their values in a variable definitions file      
#### Command
- **terraform init [options]**: command initializes a working directory containing Terraform configuration files.
- **terraform plan [options]**: command creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure.
- **terraform apply [options] [plan file]**: command executes the actions proposed in a Terraform plan
- **terraform destroy [options]**: command is a convenient way to destroy all remote objects managed by a particular Terraform configuration.

#### Run Terraform in containter:
Run docker compose: 
```dockercompose
 docker-compose run --rm terraform version
```
![23](/cicd-ws/images/2-prepair/2.3-terraform/1.png)
Run configure:

Provider (**AWS**):   **versions.tf**
```dockercompose
 terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}
```
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


Terraform init:
```dockercompose
 docker-compose run --rm terraform init
```
![23](/cicd-ws/images/2-prepair/2.3-terraform/2.png)