# linuxserver
Linux server project for Udacity FSND
# Server details
IP address: 34.211.21.189
SSH port: 2200
URL: http://34.211.21.189/
iii. A summary of software you installed and configuration changes made.
# Software installed:
* apache2
* libapache2-mod-wsgi 
* python-dev
* python-pip
* python-psycopg2 
* python-flask
* python-sqlalchemy
* postgresql
* oauth2client
* requests
* httplib2

# Summary of changes made:
## Added user grader to sudo group
Add the user to the following admin group.  This will require a sudo group (IE: Ubuntu 16.04).
```
sudo adduser grader
usermod -ag sudo grader
```
## Make sure installed packages are up to date
```
apt-get update
apt-get upgrade
```
This will update the packages and then install the upgraded versions

## Create SSH keys for the user grader
## Secure server by disabling root login
There are a few steps required to make this happen.  You will need to make changes to ```/etc/ssh/sshd_config```
Change ```PermitRootLogin without-password``` to ```PermitRootLogin no```
After this change has been made, do a restart with ```service ssh restart``` to ensure they take effect

## Change the SSH port from 22 to 2200
By default your port to access the linux bos is 22.  To change this you will need to make changes to ```/etc/ssh/sshd_config``` and change the line ```Port 22``` to ```Port 2200```.

restart the ssh service to the changed to take effect with ```sudo service ssh restart```

After the server is restarted, Grader will use: ```ssh grader@34.211.21.189 -p 2200 -i ~/.ssh/linuxCourse```

## Changed config of the UFW

Took the following steps to:
* Block all incoming connections on all ports
* Allow outgoing connection on all ports
* Allow for incoming connections for SSH on port: 2200
* Allow incoming connections for HTTP on port 80
* Allow incoming connection for NTP on port 123
* Enable firewall after changes have been made
* Check status of the firewall

This list can be acomplished with the following series of commands
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw enable
sudo ufw status
```
## NOTE: IF YOU DO THESE CONFIGURATIONS INCORRECTLY, YOU WILL LOSE ACCESS TO YOUR SERVER

## Install Apache to handle the application
Here I installed a few packages listed above, and utilized the digital ocean site to get everything set up properly.
the packages installed are:
```
sudo apt-get install apache2
sudo apt-get install libapche2-mod-wsgi
```

# Install and configure PostgreSQL for database
Here I changed my application from using SQLite to PostgreSQL
* Installed PostgreSQL
* Create user called catalog
* Enter password
* Create database called parks
* Run setup commands to pre-populate DB with information
This list can be acomplished with the following commands:
```
sudo apt-get install postgresql
sudo -u postgres createuser -P catalog
sudo -u postgres createdb parks
sudo python database_setup.py
sudo python lotsofparks.py
```

# Other changes include:
Clone repo that contains the park-picker app
* Moved to /parkpicker and cloned the repo

Updated pathing
* Updated pathing in .wsgi, .conf, and some project files to allow for proper pathing

Updated Gooogle's credentials
* Updated google OAuth secrets file and "Authorized JavaScript Origins" under Google Developers Console

Configure Apache2 to serve the app
* Created a virtual host config filw
* located at /etc/apache2/sites-available/ called parkpicker.conf

# The following resources were utilized for the completion of this project.
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
google.com
stackoverflow.comf
