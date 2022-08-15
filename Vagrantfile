# vagrant

Vagrant.configure("2") do |config|



 config.vm.box = "ubuntu/xenial64" # Linux - ubuntu 16.04

# creating a virtual machine ubuntu 
 config.vm.network "private_network", ip:"192.168.10.100"
# once you have added private network, you need to reboot VM - vagrant reload or vagrant up(vagrant destroy)
# if reload does not work - try - vagrant destroy - then vagrant up

# lets sync our app folder from localhost to VM
 config.vm.synced_folder ".", "/home/vagrant/app"
# sync data from localhost destination
 config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update 
     sudo apt-get upgrade -y
     sudo apt-get install nginx -y
     sudo systemctl enable nginx -y
     sudo apt-get install nodejs -y
     curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
     cd app
     cd app
     npm install pm2 -y
     npm install express -y
     npm install mongoose -y
     npm install -y
     npm start -y
     

 



SHELL
end
