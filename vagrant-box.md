# 🚀 Vagrant 

This guide walks through the setup of **Vagrant box with VirtualBox** on **Red Hat Enterprise Linux 9 (RHEL 9)**, including **user creation, VirtualBox Guest Additions, SSH configuration, and Vagrant box creation**.

---

## **🔹 Step 1: Install SO**
Install Redhat9 in virtualbox

## **🔹 Step 2: Create Vagrant requirements**

### Create vagrant user with sudo perms

Run the following commands to create a **Vagrant user** with passwordless sudo access:

```
sudo useradd -m -s /bin/bash vagrant
echo "vagrant:vagrant" | sudo chpasswd
sudo usermod -aG wheel vagrant  
````
For RedHat ensure sudoers config
````
wheel ALL=(ALL) NOPASSWD: ALL
````
### Install VirtualBoxLinuxAdditions

````
sudo mkdir /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
sudo ./VBoxLinuxAdditions.run
````

### Consifig ssh keys 

````
sudo mkdir -p /home/vagrant/.ssh
sudo chmod 700 /home/vagrant/.ssh
sudo curl -fsSL https://raw.githubusercontent.com/hashicorp/vagrant/main/keys/vagrant.pub -o /home/vagrant/.ssh/authorized_keys
sudo chmod 600 /home/vagrant/.ssh/authorized_keys
sudo chown -R vagrant:vagrant /home/vagrant/.ssh
````

## **🔹 Step 3: Find UUID in VirtualBox for the VirtualMachine**

To list it:````"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list vms````
To delete it: ````VBoxManage unregistervm "redhat9" --delete````

## **🔹 Step 4: Create vagrant box**

### Optional - export VirtualBox Machine

````VBoxManage export "redhat9" -o redhat9.ova````

### Create vagrant box
````vagrant package --base "redhat9" --output redhat9.box````

## **🔹 Step 5: Add vagrant box**

To add box: ````vagrant box add redhat9 redhat9.box````
To add list: ````vagrant box list````


## **🔹 Step 6: create Vagrantfile**
````
vagrant init server-rh9
Vagrant.configure("2") do |config|
  config.vm.box = "redhat9"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end
end
````

For a cluster

````
Vagrant.configure("2") do |config|
  # Define a single host-only network to prevent DHCP conflicts
  config.vm.network "private_network", ip: "192.168.56.10", virtualbox__intnet: "vboxnet0"

  # Configuración del primer servidor (DBSERVER1)
  config.vm.define "dbserver1" do |db1|
    db1.vm.box = "redhat95"
    db1.vm.hostname = "DBSERVER1"

    # Primera interfaz: NAT para acceso a Internet
    db1.vm.network "public_network", bridge: "en0: Wi-Fi (Wireless)" # NAT for Internet access

    # Segunda interfaz: Red privada con IP fija
    db1.vm.network "private_network", ip: "192.168.56.101"

    db1.vm.synced_folder ".", "/vagrant"

    db1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2

      # Define NAT and Host-Only interfaces explicitly
      # vb.customize ["modifyvm", :id, "--nic1", "nat"]
      # vb.customize ["modifyvm", :id, "--nic2", "hostonly"]
    end

    db1.vm.provision "shell", path: "provision/setup.sh"
  end

  # Configuración del segundo servidor (DBSERVER2)
  config.vm.define "dbserver2" do |db2|
    db2.vm.box = "redhat95"
    db2.vm.hostname = "DBSERVER2"

    # Primera interfaz: NAT para acceso a Internet
    db2.vm.network "public_network", bridge: "en0: Wi-Fi (Wireless)" # NAT for Internet access

    # Segunda interfaz: Red privada con IP fija
    db2.vm.network "private_network", ip: "192.168.56.102"

    db2.vm.synced_folder ".", "/vagrant"

    db2.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2

      # Define NAT and Host-Only interfaces explicitly
      # vb.customize ["modifyvm", :id, "--nic1", "nat"]
      # vb.customize ["modifyvm", :id, "--nic2", "hostonly"]
    end

    db2.vm.provision "shell", path: "provision/setup.sh"
  end
end


````
