---
title : "IaC"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4.1 </b> "
---
### Clean up resources

We will process to clearn up all the resources

#### Terraform:
Run docker compose: 
```dockercompose
 docker-compose run --rm terraform destroy --auto-approve
```

![41](/cicd-ws/images/4-cleanup/4.1-ec2/1.png)

#### AWS Checking
![41](/cicd-ws/images/4-cleanup/4.1-ec2/2.png)