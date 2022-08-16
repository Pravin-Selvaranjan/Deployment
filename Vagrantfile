#vagrant



Vagrant.configure("2") do |config|



    config.vm.define "app" do |app|

        app.vm.box = "ubuntu/xenial64" # Linux - ubuntu 16.04

        # creating a virtual machine ubuntu

        app.vm.network "private_network", ip:"192.168.10.100"

        #once you added private network, need to reboot vm

        app.vm.synced_folder ".", "/home/vagrant/app"

        # sync data from localhost

        # Shell provisioning to handle all dependencies and automate app start

        app.vm.provision "shell", inline: <<-SCRIPT

            sudo apt-get update && sudo apt-get upgrade -y

            sudo apt-get install nginx -y

            sudo systemctl enable ngninx

            curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

            sudo apt-get install -y nodejs

            cd app

            cd app

            npm install pm2 -g -y

            npm install express -y

            npm install mongoose -y


            SCRIPT

    end



    config.vm.define "db" do |db|

        db.vm.box = "ubuntu/xenial64"

        db.vm.network "private_network", ip: "192.168.10.150"

    end



end