# Creating Multiple Virtual Environments 
We begin by ensuring our vagrantfile contained within our folder has all the neccesary functions implemented.

```
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

            sudo apt-get update && sudo apt-get upgrade -y  # this updates and upgrades

            sudo apt-get install nginx -y # installs nginx

            sudo systemctl enable ngninx 
            # this is important it enables nginx just    because its installed doesnt mean it will work

            curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

            sudo apt-get install -y nodejs
            # the above two commands install the CORRECT version of nodejs for tests to pass in this particular instance 6.x

            cd app

            cd app
            # the above two commands move to the correct location

            npm install pm2 -g -y # installs pm2

            npm install express -y # installs express

            npm install mongoose -y # installs mongoose


            SCRIPT

    end



    config.vm.define "db" do |db|

        db.vm.box = "ubuntu/xenial64"

        db.vm.network "private_network", ip: "192.168.10.150"

    end



end

```

- Once our vagrantfile is correctly configured we can `vagrant up` to get everything started
- If this does not work for you using the above code please run `vagrant up app` and ` vagrant up db` respectively
- That should take a few minutes depending on your system build
- Then run `vagrant status` and you should see the below 

<img width="258" alt="vagrantstatus" src="https://user-images.githubusercontent.com/110179866/184926330-5fae4147-c9c8-4d61-a032-cae49a599026.png">




- Now you are happy both VM's are running
- We can update and upgrade both so:-
- `sudo apt-get update -y` and `sudo apt-get upgrade -y`
- Be sure to go into and come out of BOTH VM's seperately using `vagrant ssh app` and `vagrant ssh db` respectively

<img width="242" alt="vagrantssh2" src="https://user-images.githubusercontent.com/110179866/184926392-f5639c7b-c721-451c-94a5-730a49fdcae0.png">



- Use `exit` to come out of any VM

- Next we want to go into the `app` vm use `vagrant ssh app` from witin the folder on your localhost
- once in the vm `app` we want to use `npm start` and if you havent already had it installed `npm install` before start

- This will now allow us to go to the following ip: `192.168.10.100:3000`, however using reverse proxy I will be able to navigate to the app homepage without using `:3000`

- please see steps above re using nginx as a reverse proxy to allow for this

# Mongodb config

### Required dependencies 

- From within the DB VM

- `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927`

- `echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list`

- run update and upgrade `sudo apt-get update -y`
- `sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20`
- check the staatus of mongodb `systemctl status mongod`
- restart and enable the db `sudo systemctl restart mongod` `sudo systemctl enable mongod`
- `systemctl status mongod`
- `sudo nano mongod.conf`
- change bindip `0.0.0.0` allow all access
- Back to APP vm
- ensure bash file is updated `DB_HOST="AFFASF"`



