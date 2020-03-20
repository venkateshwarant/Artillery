# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "artillery"
  
  ENV['LC_ALL']="en_US.UTF-8"

  config.vm.network "private_network", ip: "192.168.33.16"

   config.vm.provider "virtualbox" do |vb|

        vb.memory = "8192"
    	vb.cpus = 4
   end

  
  config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     sudo apt-get install git-core curl build-essential openssl libssl-dev python -y
     sudo apt install git -y
     git --version
     sudo apt-get install nodejs-legacy
     sudo apt-get install npm -y
     node -v
     npm -v
     npm install -g artillery -y
     artillery -V
     sudo apt install maven  -y
     mvn -version
     git clone https://github.com/venkateshwarant/Artillery_Tutorial.git
     cd Artillery_Tutorial/
     mvn test
     
     SHELL
end
