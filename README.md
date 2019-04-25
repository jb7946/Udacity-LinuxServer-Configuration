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
4.
