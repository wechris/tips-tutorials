---
layout: post
title: Setup OwnCloud on Banana Pi 
description: Build your own cloud server.
date: 2017-12-18 20:44:00+0100
date_modified: 2017-12-18 21:44:00+0100
categories: [bananapi, owncloud, linux]
tags:
  - bananapi
  - owncloud
  - linux
author: wechris
image:
  path: /img/2017-12-18-owncloud-bananapi/image1.jpg
  width: 629
  height: 600
---

Setup OwnCloud on Banana Pi
===========================

{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image1.jpg" title="VirtualBox" width="500" %}
========================================================================

Connect your Banana Pi to the Network via a wired connection or via WLAN
and open a terminal.
:banana: :cloud: :penguin:

Update the operation system
---------------------------

To update the operating system run the commands below.

 ```bash
sudo apt-get update
 ```

If the Raspbian is up to date, we can start to install the applications.

Install the web server and PHP packages
---------------------------------------

Install the web server and PHP packages

```bash
sudo apt-get install apache2 php5 php5-gd php-xml-parser php5-intl
sudo apt-get install php5-sqlite php5-mysql smbclient curl libcurl3 php5-curl                                           
sudo apt-get install sqlite                                  
```

Add the www-data user to the www-data group

```bash
sudo usermod -a -G www-data www-data
```

Now the web server is ready and running.

Open your Browser and enter your ip-address or localhost of your Banan
Pi. You should see the Apache2 starting page.

Download and install the latest OwnCloud image
----------------------------------------------

Visit the OwnCloud installation page
[https://ownCloud.org/install/\#edition](https://ownCloud.org/install/#edition).
On the right side "Download OwnCloud server" select the download link
and copy the address of the .tar.bz2 file to install OwnCloud on your
Pi. 

Download OwnCloud server on the Pi from the official
{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image2.png" title="VirtualBox" width="500" %}

Back on the terminal, download the OwnCloud image file with the wget
command:
```bash
wget https://download.owncloud.org/community/owncloud-10.0.4.tar.bz2
```

NOTE! The link may be changed so you'd better either check it online
yourself or simply download the archive from the website directly to
your Banana Pi via browser.

After the download is finished, unpack the file to the target directory
of your web server.

```bash
cd /var/www/html                              
                                                  
sudo tar xfj /home/pi/owncloud-10.0.4.tar.bz2 
```

Now, make sure that Apache owns all of those files you just added.

```bash
sudo chown -R www-data:www-data /var/www/html
 ``` 

Create the Data Directory                                    You must create a "data" folder for Owncloud and set permissions.     
Proceed as follows.                                         
 
 ```bash
sudo mkdir /var/www/html/owncloud/data                      
sudo chown www-data:www-data /var/www/html/owncloud/data 
sudo chmod 750 /var/www/html/owncloud/data              
 ```

Finally, restart the web server

  \$ sudo service apache2 restart
  ---------------------------------

Test the own cloud web page
---------------------------

Now test your installation, by visiting your Banana Pi's URL.\
**http://\[Your IP ADDRESS\]/owncloud**.\
You should be greeted with an installation wizard like this

{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image3.png" title="VirtualBox" width="500" %}

Configure OwnCloud on your Pi
-----------------------------

First, you have to define a user name and a password for the admin
account.

In the next step, you can configure your database system. You have the
choice between SQLite3 and the MYSQL/MariaDB database system.

Click on "Storage & database" and new form field should open.

{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image4.png" title="VirtualBox" width="500" %}

By default, the SQLite is selected in the "Configure the database"
field. In the „Data folder" field, you can enter the path where SQLite
should save your data files. This is "/var/www/html/ownCloud/data" by
default. I moved my Owncloud data directory, because I'm using a SSD on
my Banana Pi.

Select the SQLite database again and finish the setup procedure with a
click on the "Finish setup" button. If everything is ok, you will be
forwarded to the login page.

{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image5.png" title="VirtualBox" width="500" %}

Speaking about another option for SQL databases, there is also MySQL
available for your choice. The general difference between the two is in
the number of users you're going to operate your cloud storage with. If
your deployment isn't likely to exceed 15-20 users, so SQLite is highly
recommended as it's much easier to use.

I use the cloud form y personal goals. I configured 3 users and 1 admin
user to connect all our devices to owncloud.

Finish the OwnCloud setup
-------------------------

Now enter your new user name and password you have created before and
you are logged into the admin area of your OwnCloud Server.

{% include lightbox.html src="/img/2017-12-18-owncloud-bananapi/image6.png" title="VirtualBox" width="500" %}

From here, you can copy and paste files, create directories, create
users and prepare/share your files and directories.

For detailed information about all the functions of OwnCloud, see the
OwnCloud UserManual.pdf which is already in your main directory.

Pimp up Owncloud
----------------

Upload a file to Owncloud as follows. Click on the square button
containing a plus sign ("+") near the top of the page. A drop down menu
appears. Choose the first menu item, "Upload", and select a file. The
file will be transferred and you should see it appear in the Owncloud
list of files. However, if the file was more than 2 MB in size, it will
not upload. Instead a message will appear at the top of the screen --
"Total file size XX MB exceeds upload limit 2 MB".

By default, the maximum size of file that can be uploaded is 2 MB, which
is not enough for many people, Increase it as follows.

Edit the file /etc/php5/apache2/php.ini:
```bash
sudo vi /etc/php5/apache2/php.ini
```
Change these two lines:\
post\_max\_size = 8M\
upload\_max\_filesize = 2M

to:

post\_max\_size = 1024M\
upload\_max\_filesize = 1024M

and save the file.

Restart Apache:
```bash
sudo service apache2 restart
```

And refresh your browser page. The upload limit should have increased.
