---
title : "Workflow"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.6.1 </b> "
---

#### Overview

**Docker backend**:
1. Chọn image đúng version dự án
2. Chọn image từ các nguồn offical, verified, sponsored
3. Chọn base image là alpine (nhẹ)

Project folder: /data/shoeshop

Create Database:
- apt install mariadb-server
- systemctl stop mariadb
- apt install net-tools
- nano /etc/mysql/mariadb.conf.d/50-server.cnf
  - bin-address = 0.0.0.0
- systemctl restart mariadb
- netstat -tlpun

Database configuration:
- mysql -u root
  