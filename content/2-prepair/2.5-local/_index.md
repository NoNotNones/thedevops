---
title : "Local"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

#### Overview
We will use local environment with **Vagrant** and **VirtualBox** to test best practices.

##### Vagrant
- Vagrant is an open-source tool for building and managing virtualized development environments. It helps developers create and configure reproducible and portable development environments that closely mimic production setups.
##### Virtualbox
- VirtualBox is a powerful open-source virtualization software developed by Oracle Corporation. It allows users to run multiple guest operating systems (OS) simultaneously on a single physical machine.

#### Configuration
**Requirement**
- Machine 1: **Linux-server**
  - IP: 192.168.33.100
  - Memory: 2048 Mb
- Machine 2: **Jenkins-server**
  - IP: 192.168.33.110
  - Memory: 5120 Mb
- Machine 3: **Monitor-server**
  - IP: 192.168.33.120
  - Memory: 2048 Mb
  
**Vagrantfile**
```vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/focal64"

  # Configuration for the first virtual machine
  config.vm.define "machine1" do |machine1|
    machine1.vm.network "private_network", ip: "192.168.33.100"
    machine1.vm.hostname = "linux-server"
    machine1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
    machine1.vm.synced_folder "./datas", "/vagrant_data"    
  end

  # Configuration for the second virtual machine
  config.vm.define "machine2" do |machine2|
    machine2.vm.network "private_network", ip: "192.168.33.110"
    machine2.vm.hostname = "jenkins-server"
    machine2.vm.provider "virtualbox" do |vb|
      vb.memory = "5120"
    end
    machine2.vm.synced_folder "./datas", "/vagrant_data"
  end

  # Configuration for the third virtual machine
  config.vm.define "machine3" do |machine3|
    machine3.vm.network "private_network", ip: "192.168.33.120"
    machine3.vm.hostname = "monitor-server"
    machine3.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
    machine3.vm.synced_folder "./datas", "/vagrant_data"    
    machine3.vm.provision "shell", inline: <<-SHELL
      apt-get update
    SHELL
  end

end
```
**Installation**
- **vagrant up**
![25](/cicd-ws/images/2-prepair/2.5-local/1.vagrant/1.png)

- **vagrant status**
![25](/cicd-ws/images/2-prepair/2.5-local/1.vagrant/2.png)

- **vagrant ssh** 
![25](/cicd-ws/images/2-prepair/2.5-local/1.vagrant/3.png)

- **vagrant halt** 
![25](/cicd-ws/images/2-prepair/2.5-local/1.vagrant/4.png)
