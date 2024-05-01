---
title : "ELK Stack"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.3.3.4 </b> "
---

#### Overview
- The ELK Stack stands for Elasticsearch, Logstash, and Kibana, which is a powerful set of open-source tools commonly used for log management and analytics.
    - **Elasticsearch**: This is a distributed, RESTful search and analytics engine designed for horizontal scalability, reliability, and real-time search capabilities. It's providing storage and indexing functionality for logs and other data. It's highly efficient for searching and analyzing large volumes of data in near real-time.
    - **Logstash**: Logstash is a data processing pipeline that ingests, transforms, and enriches data before it's indexed into Elasticsearch. It supports a wide range of input sources, including logs, metrics, events, and other structured or unstructured data formats. Logstash can perform various operations on the data, such as parsing, filtering, and formatting, to make it suitable for analysis.
    - **Kibana**: Kibana is the visualization and user interface component of the ELK Stack. It provides a web interface for users to visualize and explore data stored in Elasticsearch.Kibana offers various visualization options, including charts, graphs, maps, and dashboards, allowing users to create custom visualizations and dashboards to gain insights from their data

#### Configuration
Firewall pfSense and ELK Stack
![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/00.png?featherlight=false&width=90pc)
- Review: 
  - **pfelk** is a highly customizable open-source tool for ingesting and visualizing your firewall traffic with the full power of Elasticsearch, Logstash and Kibana.
  - Git repo reference: https://github.com/pfelk/pfelk.
  - Use Docker: https://github.com/pfelk/pfelk/blob/main/install/docker.md
  
-  Logs Sever repair:
   - Disabling Swap
   ````sh
    sudo swapoff -a
    sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
   ````
   - Configuration Date/Time Zone
   ````sh
    sudo timedatectl set-timezone Asia/Ho_Chi_Minh
   ````
   - Configure Memory
   ````sh
   sudo sysctl -w vm.max_map_count=262144
   sysctl vm.max_map_count
   ````
   - Download and Install Docker - Docker Compose
   ````sh
   apt-get install docker
   apt -y install docker.io
   sudo apt-get install docker-compose
   ````
   - Download Repo : https://github.com/pfelk/pfelk
   ![31](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/1.png)

- Configuration
   - Edit credentials **.env**
   ````sh
    ELASTIC_PASSWORD=changeme
    KIBANA_PASSWORD=changeme
    LOGSTASH_PASSWORD=changeme
    LICENSE=basic
   ````
   ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/2.png)

    - Edit logstash configure: **etc/logstash/config/logstash.yml**
   ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/3.png)

    - Run docker compose: **docker-compose up -d**
   ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/4.png)

    - Settings:
      - Log-Server:
        - Import Template : **https://github.com/pfelk/pfelk/tree/main/etc/pfelk/templates**
          - pfelk-mappings-ecs
          - pfelk-ilm
          - pfelk 
          - Options: pfelk-dhcp; pfelk-nginx
        - Import Dashboard : **https://github.com/pfelk/pfelk/tree/main/etc/pfelk/dashboard**
          - 22.04-firewall.ndjson
          - Options: 22.01-unbound.ndjson; 22.01-captive.ndjson; v20.2-dhcp.ndjson
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/5.png?featherlight=false&width=90pc)
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/6.png?featherlight=false&width=90pc)

    - Firewall-Configure: sent log to log server
      - Status > System Logs > Settings
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/9.png?featherlight=false&width=90pc)

    - Dashboard view:
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/110.png?featherlight=false&width=90pc)
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/110.png?featherlight=false&width=90pc)
     ![3334](/cicd-ws/images/3-config/3.3-labs/3.3.3-monitor/3.3.3.4-elk/120.png?featherlight=false&width=90pc)
    
Documents: https://docs.google.com/spreadsheets/d/1zewwK5ikojziCHTMt8bzRvbOV1ewgV-UjozBhae1-Lw/edit#gid=274683191