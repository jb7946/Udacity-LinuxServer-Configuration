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

### Gaining Access to Server Shell
1. If you are on a windows computer, the putty software is the preferred method of direct shell access.  Following [Amazon's document on putty access](https://lightsail.aws.amazon.com/ls/docs/en/articles/lightsail-how-to-set-up-putty-to-connect-using-ssh) to gain access.

This recipe for **cereal and milk** has been passed down my family for months.

## Ingredients

 - Cereal (you can find cool cereals [here](http://www.example.com/coolcereals))
 - Milk

## Directions

If I were writing these out as _code_, it might look something like this:

```
if bowl is empty:
    add cereal
if bowl only has cereal in it:
    add milk
```
