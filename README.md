# Linux Server Configuration

## Description
This is a description of the setup of an Amazon lightsail virtual linux server to deploy a lightweight python application.

## Access information
 - Amazon Lightsail static host ip: **3.14.103.186**
 - web link for item catalog [http://3.14.103.186](http://3.14.103.186)
 - ssh port enabled for grader user:  2200

## Provisioning Process
### Activating Amazon Lightsail server
1. Vist https://aws.amazon.com/
2. Create an account
3. Sign into the account console https://aws.amazon.com/console/
4. Under the All Services section find the link to [Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/instances), click to go to the lightsail service.
5. Please note, Amazon has their own [documentation on requesting instances](https://lightsail.aws.amazon.com/ls/docs/en/articles/getting-started-with-amazon-lightsail).  However, I will provide steps I took as follows.  Click button to create instance.
6. Under select a platform, choose Linux/Unix
7. Under select a blueprint, choose OS Only and select ubuntu option.  preferably 16.04 or lower of version numbers available to maximize compatibility.
8. It is required to select a payment plan.  For educational purposes the first month is free on the lowest plan.  You may be required to enter billing information.  Don't forget to cancel your account after completing the course if you don't intend to use it.
9. Click create instance button at bottom of page.
10. You will name the instance. Choose any name you like.  The name is not critical, but just informative for your benefit.  You may also be required to choose a locale for your instance, so be sure to choose a locale that your country or area allows access to use.
11. Once instance is created, visit [lightsail instances page](https://lightsail.aws.amazon.com/ls/webapp/home/instances).
12. Ensure your newly created instance is started.
13. Visit the [instance networking page](https://lightsail.aws.amazon.com/ls/webapp/home/networking) and request a static ip address as long as that is still free.  Static ip addresses allow you to lock an address to your application and make features like defining a dns name easier should you provision servers in the future.
14. Return to [instances page](https://lightsail.aws.amazon.com/ls/webapp/home/instances) and click your instance name to open your custom instance management page. Click connect using SSH to open a web based shell prompt into the instance.

### Gaining direct SSH to your virtual server
Of course it is possible to initially use the connect using SSH button on your lightsail console and this may be needed for initial steps, but due to Udacity requiring the changing of the ssh port to 2200, it will eventually break the lightsail connect to ssh button requiring direct ssh to administrate server.
 - Initial access for ubuntu user can be obtained by jumping to your [AWS Account keys page](https://lightsail.aws.amazon.com/ls/webapp/account/keys).
 - Select the default key and download the file.  It should have a name like LightsailDefaultKey##locale##.pem where ##locale## is unique text related to where your instance was created.  Download this file and save it to a place on your computer that you can find in the future.  It will be needed to set up initial ubuntu user access.
 - From your [instance management window](https://lightsail.aws.amazon.com/ls/webapp/home/instances) click on your instance to drill into management of the instance.  you should see tabs like Connect, Storage, Metrics, Networking.
 - Click on the Networking tab.
 - In the firewall section of the networking tab, make sure the following are set up:
```
SSH TCP 22  
HTTP TCP 80
Custom UDP 123
Custom TCP 2200
```
_Please note: SSH TCP 22  will get blocked in the future due to course requirements, but it is needed initially_
#### Windows OS
If you are on a windows computer, the putty software is the preferred method of direct shell access.  Follow [Amazon's document on putty access](https://lightsail.aws.amazon.com/ls/docs/en/articles/lightsail-how-to-set-up-putty-to-connect-using-ssh) to gain access.
#### Mac OS / Linux
Following are steps I followed on Mac OS running Mohave:
1.  open a terminal window {command + T}. Control-Alt-T on Unbuntu will also open terminal.
2.  find the LightsailDefaultKey##locale##.pem file i downloaded from above section and move that file to your /Users/<loginid>/.ssh folder where <loginid> is your macos login id.  I also renamed the file to id_rsa, but if you already have an id_rsa file, just rename it to something short and easy to remember.
3.  Use the ssh command to connect to the lightsail server.  command format as follows:

    ```ssh -i <keyfilename> -p <port number> <user>@<host ip or dns>```

    * replace ```<keyfilename>``` with the name you renamed your LightsailDefaultKey##locale##.pem
    * replace ```<port number>``` with 22 for now which is default, but since this will change in the future, it is important to know how to specific the value.
    * replace ```<user>``` with ubuntu
    * replace ```<host ip or dns>``` with the static ip of your lightsail server.

4.  You should have successfully connected to the lightsail virtual server via direct ssh.

### Creating a ssh key for the Grader user, building the user and setting permissions.
This will outline steps needed to generate a key for a new user that will be provisioned in your lightsail server.
#### Windows OS
Windows does not have a key generator by default, so one must be obtained.  A good example of a keygen tool is [puttygen](https://www.ssh.com/ssh/putty/windows/puttygen).
#### Mac OS
1. open a terminal window {command + T}.
2. change to your .ssh folder for your profile. 
```cd ~/.ssh```
3. execute command ```ssh-keygen```.  I will include example:
```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/jdb/.ssh/id_rsa): grader
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in grader.
Your public key has been saved in grader.pub.
The key fingerprint is:
SHA256:################################### <user>@<domain>
The key's randomart image is:
+---[RSA 2048]----+
|      oOO@++..   |
|      o.o.%.+ +  |
|      ..+o @ = . |
|     ..o oE o    |
|    .o .S        |
|    o.+o...      |
|   o o++ o       |
|    .oo o        |
|    ..           |
+----[SHA256]-----+
```
4. I left passphrase empty, and pressed enter until the randomart image was output.
5. In your current folder, there should be two new files.
```
-rw-------  1 user  group  1831 Apr 25 00:36 grader
-rw-r--r--  1 user  group   406 Apr 25 00:36 grader.pub
```
* The grader file will be the private key which will be shared with Udacity when submitting the course.
* The grader.pub file is the public key that will be added to the lightsail server user grader for granting access.
6. Either open the grader.pub file in a text editor or execute the command ```cat grader.pub``` to list the contents and then copy that to the text editor.  this will be needed on a future step.
7. ssh into the lightsail server as outlined in previous section.  It is expected that you will be logged in as ubuntu. The next steps will be performed on the lightsail server as ubuntu user.
```ssh -i ~/.ssh/id_rsa -p 22 ubuntu@<lightsail ip address>```

#### Adding user and setting permissions on lightsail server

    * create the grader user: ```sudo adduser grader```
    * give the grader user sudo permissions:
    ```sudo nano /etc/sudoers.d/grader```
    * paste into this new file the following text:
    ```grader ALL=(ALL) NOPASSWD:ALL```
    * save the file with control-O and return
    * exit nano with control-X
8. Switch to the grader user and then switch to the grader home path.
    ```sudo su grader```
    ```cd```
9. create .ssh directory and change into new directory
    ```mkdir .ssh```
    ```cd .ssh```
10. create authorized_keys file.
    ```touch authorized_keys```
11. add the recently created grader.pub value to this new file and udpate permissions.
    * open file to edit.
    ```sudo nano authorized_keys```
    * paste the contents of the grader.pub file into this file, and then save (control-O return) and exit (control-x).
    * change permissions of authorized_keys.
    ```sudo chmod 644 authorized_keys```
    * change permissions of .ssh folder
    ```sudo chmod 700 ~/.ssh```
    * type ```exit```and return to switch back to ubuntu user.
    * type ```exit```and return again to exit ssh connection.
12. Attempt to reconnect as grader user.
```ssh -i ~/.ssh/grader -p 22 grader@<lightsail ip address>```
13. Do not proceed to next session unless your connection steps are successful.  We will be altering how to connect which may be catastrophic if the steps thus far were not successful.
### Change SSH service port from 22 to 2200 and disable root login on lightsail server
1. At this point you should be connected to the lightsail server via ssh as ubuntu or grader user.
2. execute command ```sudo nano /etc/ssh/ssdh_config```.  This will open the configuration file for the ssh service.
    * search for the line with _Port_ 22 and change it to _Port_ 2200
    * search for the line with _PermitRootLogin_ and ensure it reads no
    * Control-O return to save, and Control-X to exit
4. restart ssh service with ```sudo service ssh restart```.  This may disconnect you.  From now on your connection command will use port 2200.
```ssh -i ~/.ssh/grader -p 2200 grader@<lightsail ip address>```
### Configure the firewall on the Lightsail server (UFW)
1. Enter the following commands as logged into lightsail server with either ubuntu or grader user.
```sudo ufw disable```
```sudo ufw default deny incoming```
```sudo ufw default allow outgoing```
```sudo ufw allow 2200/tcp```
```sudo ufw allow 80/tcp```
```sudo ufw allow 123/udp```
```sudo ufw enable```
```sudo ufw status```
    * make sure your Uncomplicated Firewall output is similar to follows:
    ```
    Status: active

    To                         Action      From
    --                         ------      ----
    80                         ALLOW       Anywhere                  
    80/tcp                     ALLOW       Anywhere                  
    2200/tcp                   ALLOW       Anywhere                  
    123/udp                    ALLOW       Anywhere                  
    123                        ALLOW       Anywhere                  
    80 (v6)                    ALLOW       Anywhere (v6)             
    80/tcp (v6)                ALLOW       Anywhere (v6)             
    2200/tcp (v6)              ALLOW       Anywhere (v6)             
    123/udp (v6)               ALLOW       Anywhere (v6)             
    123 (v6)                   ALLOW       Anywhere (v6)    
    ```
### Install and configure packages on Lightsail server
1. connect to lightsail server using ssh as defined in previous steps.
2. Run the following commands to get needed packages and select Y if asked to confirm installation.

```
sudo apt-get install finger apache2 libapache2-mod-wsgi postgresql postgresql-contrib postgresql-server-dev-9.5 python2.7 python-flask python-pip httplib2 python-oauth2client virtualenv git libpq-dev python-dev
```
* for the next steps if the sudo pip install does not work, then try sudo -H pip install instead.
```
sudo pip install psycopg2
sudo pip install psycopg2-binary
sudo pip install flask
sudo pip install httplib2
sudo pip install requests
sudo pip install sqlalchemy
sudo pip install virtualenv
sudo pip install google-api-python-client oauth2client
sudo virtualenv venv
```

### Ensure Lightsail server has all needed updates and security patches

1. run the following commands from shell in the lightsail server. Type Y to accept any changes.

   > | Commands to execute in lightsail shell |
   > | ----------------------------- |
   > | ```sudo apt-get update``` |
   > | ```sudo apt-get upgrade``` |
> | ```sudo apt-get autoremove```|
   >



### Clone your item catalog into the Lightsail server

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. Ensure apache2 service is running with command.

   ```sudo service apache2 start```

2. Enable mod_wsgi with the command.

   ```sudo a2enmod wsgi```

3. Change to the default web folder for apache.  ``` cd /var/www```

4. Create catalog folder.  ```mkdir catalog```

5. Change into catalog folder. ```cd catalog```

6. Copy the item catalog project from GitHub with this command:

   ``` git clone https://github.com/jb7946/item-catalog catalog```

### Configure Web Server Gateway Interface (WSGI)

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. create wsgi config file using following command and paste in below text

   ```sudo nano /var/www/catalog/catalog.wsgi```

```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/catalog/")

from catalog import app as application
application.secret_key = 'your_secret_key'
```

* Control-O and return to save
* Control-X to exit

### Create Catalog Virtual host in Apache2

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. use this command to create a new virtual host config file that has my specific server configuration settings.  Paste the information below into this new file.
   * ```sudo nano /etc/apache2/sites-available/catalog.conf```

```
<VirtualHost *:80>
                ServerName 3.14.103.186
                ServerAdmin grader@3.14.103.186
                WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
                WSGIProcessGroup catalog
                WSGIScriptAlias / /var/www/catalog/catalog.wsgi
                <Directory /var/www/catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog/catalog/static
                <Directory /var/www/catalog/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### Configure postgreSQL database

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. Switch to the postgres user with this command ```sudo -u postgres -i```

2. Your command prompt will change to a $.  Enter command ```psql``` to get database console.

3. Enter the following commands at the database prompt and press enter for each line to send:

   ```
   create user catalog with password catalog;
   alter user catalog createdb;
   create database myitemcatalog with owner catalog;
   revoke all on schema public from public;
   grant all on schema public to catalog;
   ```

4. Exit database with \q and return.
5. use command ```exit``` to return to your primary user.

### Load Data from project into database.

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. Prior to loading data, the /var/www/catalog/catalog/database_setup.py and /var/www/catalog/catalog/database_items.py files must have a change to the engine variable so they will connect to the postgresql database instead of building a localized db.

   * in each file from step 1, find the following line

     ```engine = create_engine('sqlite:///myitemcatalog.db')```

   * and change it to the following

     ```engine = create_engine('postgresql://catalog:catalog@localhost/myitemcatalog')```

2. change directory to the database files ```cd /var/www/catalog/catalog```

3. use python to run the two files and define and populate the database.

   ```python database_setup.py
   python database_setup.py
   python database_items.py
   ```

4. The database should now be loaded with data for the catalog app.

### Reconfigure application.py 

For these steps, you must be connected to the lightsail server via ssh.  These commands are intended to be executed in the virtual server shell as ubuntu or grader.

1. in folder /var/www/catalog/catalog, rename the application.py file with the following command:

   ```mv application.py __init__.py```

2. Edit the ```__init__.py``` file with command ```sudo nano __init.py```.

   - find the following line

     ```engine = create_engine('sqlite:///myitemcatalog.db')```

   - and change it to the following

     ```engine = create_engine('postgresql://catalog:catalog@localhost/myitemcatalog')```

3. Restart apache2 server with command ```sudo service apache2 restart```

4. Enable the site with command ```sudo a2ensite catalog```

5. The application should now come up on the browser at [http://3.14.103.186](http://3.14.103.186)

### References:

* [VirtualEnv](https://virtualenv.pypa.io/en/latest/) documentation.
* Digital Ocean [tutorial](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps) on installing flask app.
* Ubuntu [documentation](https://help.ubuntu.com/16.04/ubuntu-help/index.html) on updating packages and security updates.
* Udacity Instruction materials, Knowledge Center, Student Chat
* Stack Overflow
* Amazon AWS Services