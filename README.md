# Monolithic Architecture

The monolithic architecture is considered to be a traditional way of building applications. A monolithic application is built as a single and indivisible unit. Usually, such a solution comprises a client-side user interface, a server side-application, and a database. It is unified and all the functions are managed and served in one place.

Normally, monolithic applications have one large code base and lack modularity. If developers want to update or change something, they access the same code base. So, they make changes in the whole stack at once.

![Blank diagram (2)](https://user-images.githubusercontent.com/110179866/184661404-e4f5cbf9-1190-4a9b-823b-b7865309e9c1.jpeg)



# Automating installation packages using provisioning in Vagrant

- Ensure you have either destroyed or are not running any previous instances of a VM
- use `vagrant destroy` or once completing the steps below `vagrant reload`
- within your new folder create a vagrantfile

(image)

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

(image)


- As above, you should receive 0 failures if all is running correctly

- Next, navigate inside your VM using `vagrant ssh`

(image)

- Once inside the VM, navigate to the folder where the data is stored, in this instance `vagrant@ubuntu-xenial:~/app/app$`
- You can use `ls` to confirm that you are in the correct folder as it will show you a list of all files within said folder

(image)


- From within this folder run `npm install`
- Followed by `npm start`
- This should run the programme and on your relevant ip display the app

(image)