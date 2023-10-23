## What the Repo is all About

In this repo, vagrant is used to set up two ubuntu-based servers - master and slave. Their provisioning is automated with a bash script. The bash script which automates the provisioning of the servers is used to automate the deployment of a LAMPstack. The script clones a PHP application; laravel, configures Apache web server and MySQL.

Ansible playbook is deployed on the slave node and laravel is accessible through the ip addresses of both the master and slave machine. A cron job checks the server's uptime every 12am.

### A breakdown

1. Laravel runs on lampstack

- Laravel: This is a PHP application deployed on the slave node in the virtual machine

2. Installing lampstack involves installing apache, mysql, and different versions of PHP. By using add apt repository, we're pulling the PHP repository. After pulling it, the next is to update the system so it can take note of it. The next step is to install different versions of PHP 

3. To install a PHP application which in this case is laravel, a composer, which is a dependency for laravel is installed. Curl is used to install composer, It is also used to download a package from a Web site 

The next thing is to configure apache webserver. In configuring apache webserver, copy it to port 80
    In the script, the
    Server admin - email address where the owner of the application can be reached 
    Servername - domain name or ip address 
    Document route - where the Web allocation will be hosted 
If an error comes up, it will come to the error log.
The rewrite command enables the module for apache. The laravel.conf makes everything active and after that it will be restarted


4. Master and slave: The master and slave machine is set up with vagrant. For the master-slave, vagrant must be installed, alongside a virtual box. Initialize vagrant with vagrant in it ubuntu/focal64. By initializing vagrant, a vagrantfile will be automatically created. The default of the vagrantfile will be replaced with the content of the master-slave file which has a slave server and a master server.
The Daemon line is used in the script as it allows us login to the machine using the host name.
The memory is provisioned and set to a total of 2gb and 4cpus for both of them
Vagrant up in the file will automatically run the slave and master machine once the script is run
The /dev/null is inserted to make it noninteractive so as to avoid receiving any prompts


5. Ansible play book. In this directory, another directory called the files is created. The laravel-slave.sh is here.
Three other files are in the Ansible directory 


6. Cloning laravel:
That command clones laravel and stores it in the var/www/html 

sed -i command in the .env helps to manipulate the username and password without having to use an editor like vim or nano. It just searches for the dB part and implements whatever changes have been made there.

7. For MYSQL:
A user with password and the database is created.
An argument is passed and on running the script, the name and password is appended to the script. This is flexible as anyone who enters a name and password respectively will have the database created in that name and password.

Upon pulling the PHP file, there's an .env file where adjustments need to be made - the database, username and password. These should be the same with the mysql name and password 


## Explanation of the Different Sections of the Repo 

1. The Ansible playbook is a directory created in the ALTSCHOOL-EXAM directory, it helps to run the script on the Ansible machine. The Ansible runs on the slave node

- Files: This is another directory created in the Ansible playbook directory. It contains the laravel-slave.sh file. This file installs all the laravel apps and installs the applications with lampstack and laravel

- Ansible. Cfg: This is a file created in the Ansible-playbook directory to hold the configuration of ansible

- inventory: The inventory is another file and it holds the target machine by housing the ip address. Only the ip address of the slave node is in the inventory

- site.yaml: It copies a cronjob. copies the file. Changes the command of the file and runs the bash/laravel script

- Master and slave.sh file: It spins up master and slave

- Laravel.sh file: Helps to deploy laravel on the master


## Explanation on how to Run the Script
There are 3 conditions for the script to run perfectly
- .env file: Here, change username, database and password. The username and the database must be the same

- MySQL: To run this, it must be followed by a username and password which is the same with the .env file. In this case, it is ./laravel.sh danny azoro2020

- Ansible config file: here, you change server name to your ip address. Make sure it matches the ip address of the server you're running the laravel script on

Run the master and slave first. SSH into the master user then Switch to the root user and run laravel.sh with the name and password as arguments. This will run the files. In the directory, generate an ssh key on the master machine and copy it to the slave machine inside the authorised key path. This will enable communication. Run the ansible with "ansible-playbook -i inventory site.yaml" running this updates and upgrades the server, sets the cronjob to check uptime of the server every 12am, copies the script to the slave machine, and executes the script.

![Alt text](<Ansible successfully deployed showing ok=6, changed=5.png>)![Alt text](<Master and slave virtual machines running.png>)![Alt text](<Screenshot showing laravel page for the master ip address (192.168.20.10.png>)![Alt text](<Screenshot showing laravel page for the slave ip address (192.168.20.11).png>)![Alt text](<Root server of the slave machine showing laravel directory has been created.png>)![Alt text](<Screenshot (30).png>)