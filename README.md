# Monolithic Architecture

The monolithic architecture is considered to be a traditional way of building applications. A monolithic application is built as a single and indivisible unit. Usually, such a solution comprises a client-side user interface, a server side-application, and a database. It is unified and all the functions are managed and served in one place.

Normally, monolithic applications have one large code base and lack modularity. If developers want to update or change something, they access the same code base. So, they make changes in the whole stack at once.

![Blank diagram (2)](https://user-images.githubusercontent.com/110179866/184661404-e4f5cbf9-1190-4a9b-823b-b7865309e9c1.jpeg)



# Automating installation packages using provisioning in Vagrant

- Ensure you have either destroyed or are not running any previous instances of a VM
- use `vagrant destroy` or once completing the steps below `vagrant reload`
- within your new folder create a vagrantfile


<img width="372" alt="vagrantfile" src="https://user-images.githubusercontent.com/110179866/184681966-09bfebbd-b000-46af-be52-881984255c28.png">

- The following code is neccesary in order to correctly provision the right packages in order for the app to present correctly online. 

```# vagrant

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
     sudo apt-get install -y nodejs
     cd app
     cd app
     npm install pm2 -g
     npm install express -y
     npm install mongoose -y

SHELL
end
```

- This will automate all of the packages being installed when you `vagrant up`
- Ensure to save the file before attempting `vagrant up`

- Once all processes have been completed and the command line has returned, navigate to the folder containg the test code, in this instance `/Deployment_eng122/environment/spec-tests`

- Within this directory run ` rake spec` 


<img width="790" alt="rakespec" src="https://user-images.githubusercontent.com/110179866/184682000-b7577acc-6b4b-4bb2-a6fd-717d9cc1403b.png">


- As above, you should receive 0 failures if all is running correctly

- Next, navigate inside your VM using `vagrant ssh`


<img width="293" alt="vagrantssh" src="https://user-images.githubusercontent.com/110179866/184682087-9ee60b8d-90b4-4968-a845-a7a5fc18c5dd.png">


- Once inside the VM, navigate to the folder where the data is stored, in this instance `vagrant@ubuntu-xenial:~/app/app$`
- You can use `ls` to confirm that you are in the correct folder as it will show you a list of all files within said folder



<img width="295" alt="appls" src="https://user-images.githubusercontent.com/110179866/184682116-830ca386-1cc2-4485-879c-ae6d8e9882e8.png">


- From within this folder run `npm install`
- Followed by `npm start`
- This should run the programme and on your relevant ip display the app


<img width="296" alt="npmstart" src="https://user-images.githubusercontent.com/110179866/184682145-0d812cf2-beba-49a4-9c5a-5a62f987b60d.png">




<img width="959" alt="spartapp" src="https://user-images.githubusercontent.com/110179866/184682167-0f00013e-70af-42a2-8e05-e795581df7d4.png">



![virtenv (1)](https://user-images.githubusercontent.com/110179866/185174187-8042b2b0-767c-411a-b436-11c4d853b073.jpeg)




### Linux Variable and Env Variable in Linux - Windows - Mac

- How to check existing Env Var  `env` or `printenv`
- How to create a var in Linux `Name=Pravin`
- How to check Linux Variables `echo $Name`
- Env Var we have a keyword called `export` `export Last_name=Selvaranjan`
- check specific Env var `printenv Last_name`

####### How to make Env Variable `PERSISTENT`
- research how to make env persistent of your `first_name`, `last_name` and `DB_HOST`
- `DB_HOST=mongodb://192.168.10.150:27017/posts`
-  To set permanent environment variables for a single user, edit the `.bashrc` file `sudo nano ~/.bashrc`
-  Within the file `export [VARIABLE_NAME]=[variable_value]`


## Nginx as a reverse proxy

- `sudo nano /etc/nginx/sites-available/default` to enter config file for nginx
- under `server_name` `location / {` (see below) enter proxy_pass `http://localhost:3000;`


<img width="360" alt="maiks" src="https://user-images.githubusercontent.com/110179866/184926265-e1a3875a-6190-4010-bf78-6bb3a6681321.png">



- This should now allow you to access the app without using port 3000

## Creating multiple virtual environments 
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
- 
- 


