--- 
layout: post
title: Setting up Managing News' Drupal Profile on Amazon EC2
---

Managing News is a clever implementation of Drupal. Amazon Web Services (AWS) is a clever hosting platform.

Here are the steps that worked for me to set up a test environment for Managing News on AWS EC2

1. Setup an AWS account. (there sales pitch, even though its next to free tot try)
   
    * Sign in and click the EC2 tab in the AWS console
    * Click "AMIs" in the left toolbar
    * Select "Amazon Images" from the drop down and search for Drupal. (I chose the 64bit Drupal 7.X version)
    * Setup the machine to your liking; I use all of the default and the quickstart firewall / port setting.
    * Be sure to save your *.pem key in a safe place. Linux machines default to ~/.ssh
    * Start it up (this should happen as the last step in AWS' step-wise guide)

2. Login to your new machine
   
    * Click on "Instances" in the AWS toolbar.
    * Right click on the instance we just created.
    * Connect! and follow the directions provided by AWS.
    * Test out the default Drupal installation already on the machine by going to its public url (find this in the instance information or right-click connect)

3. Setup your new Managing News Profile
  
    * The default installation of Drupal is at /var/www/html check it out and troubleshoot if you couldn't connect to the public url. Try
    <pre><code>	
    !bash
    sudo su
    /etc/init.d/httpd start
    </code></pre>
    * Once you've tested that drupal is up and running, move the installation out of the way.
    <pre><code>	
    !bash
    sudo su
    mkdir /var/www/drupal7
    mv /var/www/html/* /var/www/drupal7
    </code></pre>
    * Download Managing News
    <pre><code> 
    !bash
    mkdir managing_news
    cd ~/managing_news
    curl -C - -O http://www.managingnews.com/files/managingnews-1.2.zip
    </code></pre>
    * Install Managing news, or following these [directions](http://drupal.org/documentation/install/developers) from drupal.org
    <pre><code>
    !bash
    unzip managingnews-1.2.zip
    sudo su
    mv managingnews-1.2/* /var/www/html/
    </code></pre>
        * Go to your machines public url
        * Select "Managing News" as the installation profile.
        * Follow the directions to  create a settings.php file. Here's mine:
    <pre><code>
    !bash
    sudo cd /var/www/html/sites/default
    sudo cp default.settings.php settings.php
    </code></pre>
    * Set up MySql server
        * unfortunately, the mysql server seems to be a bust on the AMI. Let's reinstall it with help from this [tutorial](http://getablogger.blogspot.com/2009/05/how-to-install-mysql-in-centos-53-fro...
    <pre><code>    
    !bash
    sudo yum remove mysql-server
    sudo yum remove mysql
    sudo yum install mysql-server
    sudo yum install mysql-devel
    sudo yum install php-mysql
    </code></pre>
    * Set up phpMyAdmin, a useful Graphical Interface to help with the SQL illiterate
    <pre><code>
        sudo yum install phpmyadmin
        sudo vim /etc/httpd/conf.d/phpMyAdmin.conf
        #add this line with your own ip address in order to tell apache to let you through to phpMyAdmin
        Allow from your.ip.address.here
        # you should really set up SSL for phpMyAdmin, but that's another tutorial.
    </code></pre>
        * Go to http://publicDNS.com/phpmyadmin. Add a user and database for Managing news. I called mine "mn_test"

    * Fix the PHP strict timezone warnings [(issue ticket)](http://drupal.org/node/325827) [(solution)](http://groups.drupal.org/node/17970#comment-179313)
     <pre><code>      
         #!bash
         sudo vim /var/www/html/sites/default/settings.php
         #add the following line under php settings
         ini_set('date.timezone', date_default_timezone_get());
     </code></pre>
    * Finally, enable Clean Urls to get rid of the "q?=" etc in your urls. [Help](http://drupal.org/node/15365)
    <pre><code>     
        #!bash
        sudo vim /etc/httpd/conf/mn.conf
        #paste this
      <Directory /var/www/html>
         RewriteEngine on
         RewriteBase /
         RewriteCond %{REQUEST_FILENAME} !-f
         RewriteCond %{REQUEST_FILENAME} !-d
         RewriteRule ^(.*)$ index.php?q=$1 [L,QSA]
      </Directory>
    </code></pre>

That's all. Go, now, save the world.

