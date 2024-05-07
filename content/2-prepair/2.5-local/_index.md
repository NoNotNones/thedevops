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
{{%expand "Expand:" %}}
```sh
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
{{% /expand%}}

**Run**

{{%expand " vagrant up " %}}
![25](/thedevops/images/2-prepair/2.5-local/1.vagrant/1.png)
{{% /expand%}}

{{%expand " vagrant status " %}}
![25](/thedevops/images/2-prepair/2.5-local/1.vagrant/2.png)
{{% /expand%}}


{{%expand " vagrant ssh " %}}
![25](/thedevops/images/2-prepair/2.5-local/1.vagrant/3.png)
{{% /expand%}}

{{%expand " vagrant halt" %}}
![25](/thedevops/images/2-prepair/2.5-local/1.vagrant/4.png)
{{% /expand%}}

{{%expand " vagrant reload" %}}
for upgrade Ram, CPU
{{% /expand%}}


{{%expand " vagrant destroy -f" %}}
Destroy all machine
{{% /expand%}}

### Windows WSL & Vagrant

[Source review](https://blog.thenets.org/how-to-run-vagrant-on-wsl-2/)

- Requirements:
  - Windows 10
  - Virtualbox
  - WSL 2
  - Vagrant
  - Vagrant plugin: vitualbox_WSL2

- Install VirtualBox
- Install WSL2: `wsl -l -v`
- Install Powershell Preview
````js
Invoke-Expression "& { $(Invoke-Restmethod https://aka.ms/install-powershell.ps1) } -UseMSI -Preview"
````
- Install Vagrant (PS)
````sh
# run inside WSL 2
# check https://www.vagrantup.com/downloads for more info

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
````

- Update Vagrant (PS): 

````js
vagrant --version
choco install vagrant --version 2.4.1
````

- Enable WSL 2 support (The Terminal on WSL2)

````sh
# append those two lines into ~/.bashrc
echo 'export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"' >> ~/.bashrc
echo 'export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"' >> ~/.bashrc

# now reload the ~/.bashrc file
source ~/.bashrc
````

- Install virtualbox_WSL2 plugin (The Terminal on WSL2)

````sh
# Install virtualbox_WSL2 plugin
vagrant plugin install virtualbox_WSL2
````

- Install and configured (The Terminal on WSL2)

````sh
# Go to Windows user's dir from WSL
cd /mnt/c/Users/<my-user-name>/

# Create a project dir
mkdir -p projects/vagrant-demo
cd projects/vagrant-demo

# Create a Vagrantfile using Vagrant CLI
vagrant init hashicorp/bionic64
ls -l Vagrantfile

# Start a VM using Vagrantfile
vagrant up

# Login to the VM
# (password is 'vagrant')
vagrant ssh

# Done :)
````

{{%expand "Processing" %}}
![24](/thedevops/images/2-prepair/2.5-local/1.png)
{{% /expand%}}

#### Vagrantfile 

{{%expand "Vagrantfile (WSL): " %}}

```sh
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Machine 1 configuration
  config.vm.define "machine1" do |machine1|
    machine1.vm.box = "hashicorp/bionic64"
    machine1.vm.hostname = "machine1"
    machine1.vm.network "private_network", ip: "192.168.56.101"
    machine1.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  # Machine 2 configuration
  config.vm.define "machine2" do |machine2|
    machine2.vm.box = "hashicorp/bionic64"
    machine2.vm.hostname = "machine2"
    machine2.vm.network "private_network", ip: "192.168.56.102"
    machine2.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end
end

```
{{% /expand%}}

- vagrant ssh machine1
- password: vagrant
