## Create a Vagrant Project Directory

Create a new project directory and navigate into it:

```bash
mkdir haproxy-loadbalancing-lab
cd haproxy-loadbalancing-lab
```
---

## **Initialize a Vagrant file for UTM**

Generate a Vagrantfile in project directory by running

```bash
vagrant init
```
The vagrant file defines the virtual machine default settings
---
## **Edit the vagrant file for UTM Compatibility**

To open the vagrantfile for editin:

```bash
nano Vagrantfile
```
Replace the contents with the following configuration to set up three VMs: `Web Server 1`, `Web Server 2`, and `HAProxy`.

```ruby
Vagrant.configure("2") do |config|
  # Use UTM provider for Vagrant
  config.vm.provider :qemu do |qemu|
      qemu.memory = 1024 # RAM in MB
      qemu.cpus = 1 # Number of CPU cores
  end


  # Web Server 1
  config.vm.define "web1" do |web1|
    web1.vm.box = "cloud-image/ubuntu-22.04"
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.64.10"
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      echo "<h1>Welcome to Web Server 1</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Web Server 2
  config.vm.define "web2" do |web2|
    web2.vm.box = "cloud-image/ubuntu-22.04"
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.64.11"
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get install -y apache2
      echo "<h1>Welcome to Web Server 2</h2>" | sudo tee /var/www/html/index.html
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # HAProxy Load Balancer
  config.vm.define "haproxy" do |haproxy|
    haproxy.vm.box = "cloud-image/ubuntu-22.04"
    haproxy.vm.hostname = "haproxy"
    haproxy.vm.network "private_network", ip: "192.168.64.12"
    haproxy.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y haproxy
      sudo systemctl enable haproxy
      sudo systemctl start haproxy
    SHELL
  end
end
```
Save and exit.

---

## Edit the vagrantfile for UTM Compatibility

Edit the Vagrantfile to set up three VMs: Web Server 1, Web Server 2, and HAProxy. The following configurations were added:

- **Web Server 1:** IP: 192.168.64.10
- **Web Server 2:** IP: 192.186.64.11
- **HAProxy:** IP: 192.168.64.12

The Vagrantfile contents ensures a configuration for each VM to have it's own IP address and provisoin Apache for the web servers and HAProxy for load balancing.

---

## Download the Ubuntu box for UTM

To download the Ubuntu 22.04 box, i ran:

```bash
vagrant box add cloud-image/ubuntu-22.04 --provider=utm
```
- Finding a provider almost proved to be an issue for me as vagrant does not have alot of utm support. i had to look for a box which supported vagrant for me as it does not support my provider (utm).
- I ran the command `vagrant cloud search ubuntu-22.04` to look for a box compatible. there were alot of compatible boxes for virtual box, i did not see any for UTM. Although, i saw one which had a `QEMU` support, and this worked since UTM is based on qemu.

- In my Vagrant file i adjusted the provider from `utm` to `libvert` and settled at `qemu` which worked since that's the box available for qemu

replacing for this line in **Vagrantfile**
```bash
config.vm.provider :qemu do |qemu|
  qemu.memory = 1024 # RAM in MB
  qemu.cpus = 1 
```
i also installed the `vagrant-libvirt` plugin, and uninstalled the `vagrant-utm` plugin since we cant use it
---


