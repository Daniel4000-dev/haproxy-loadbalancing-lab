# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
   # Use UTM provider for Vagrant
   config.vm.provider :qemu do |qemu|
    qemu.memory = 1024 # RAM in MB
    qemu.cpus = 1 # Number of CPU cores
    end


    # Web Server 1
    config.vm.define "web1" do |web1|
      web1.vm.box = "bento/ubuntu-24.04"
      web1.vm.box_version = "0.0.1"
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
      web2.vm.box = "bento/ubuntu-24.04"
      web2.vm.box_version = "0.0.1"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.64.11"
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        echo "<h1>Welcome to Web Server 2</h2>" | sudo tee /var/www/html/index.html
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end

    # HAProxy Load Balancer
    config.vm.define "haproxy" do |haproxy|
      haproxy.vm.box = "bento/ubuntu-24.04"
      haproxy.vm.box_version = "0.0.1"
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
