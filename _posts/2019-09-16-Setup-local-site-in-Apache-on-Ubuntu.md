---
layout: post
title: How to Use Apache Virtual Host to Run Multiple Local Websites on Ubuntu 16.04 
date: 2018-09-16 21:37:00 +0300
description: How to Use Apache Virtual Host to Run Multiple Local Websites on Ubuntu 16.04 
img: Apache-http-server.png # Add image post (optional)
tags: [Apache, Ubuntu, Linux, website, localhost] # add tag
---

# Introduction and Requirements 

In this article, we will learn how to host multiple virtual websites with Apache on Ubuntu 16.04. I am assuming that you already have Apache server up and running but if you don't, just run ``sudo apt-get install apache2`` to install it on Ubuntu. A running Apache web server is all you need for this tutorial, but if you want actually build something in it (i.e, a nice website), then you might want to check out "LAMP stack" on Google. I also have an Ansible playbook for installing LAMP stack and can be seen in [here](https://github.com/armagankaratosun/ansible/tree/master/ansible-lamp). Now, let's get down to business!

### 1) Create Vhost Configuration file for Apache

In order to host your website (in local or not), you have to create configuration files for Apache in related directories. To do that, create a file with the name you desired, ``example1.com.conf`` in this example, in ``/etc/apache2/sites-available`` and add the following lines;

```bash
<VirtualHost *:80>
	ServerAdmin root@localhost
    ServerName example1.localhost
	DocumentRoot "/home/$USER/Desktop/example1.com/public_html"
	<Directory "/home/$USER/Desktop/example1.com/public_html">
	        Order allow,deny
	        Require all granted
	        Allow from 127.0.0.1
		AllowOverride All
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/example1.com_error.log
	CustomLog ${APACHE_LOG_DIR}/example1.com_access.log combined
</VirtualHost>
```
In here, 

* ```ServerAdmin``` = contact info of the admin.
* ```ServerName```  = name of your locally hosted website.
* ```DocumentRoot``` = is the root directory for your webiste.
* ```Directory``` = is used to enclose a group of directives that will apply.
* ```ErrorLog``` = is where error logs are stored.
* ```CustomLog``` = is where access logs are stored.
* ```$USER``` = is your username, change it with your actual username.

You can modify this configuration file as you wish, but just make sure that the file is owned by the root user and group.

### 2) Make Your Site Available with ```a2ensite```

We have to copy our new configuration file to ```/etc/apache2/sites-enabled``` directory to enable it but in Apache, we have tool for that called ```a2ensite```. While you making your site available with ```a2ensite```, you might want to disable default Apache web page and configuration with ``a2dissite`` and reload the Apache configuration. So in summary;

* ``sudo a2ensite example1.com.conf`` to enable your site.
* Disable default Apache web page and configuration with ``sudo a2dissite 000-default``
* ``sudo service apache2 reload`` (or ``sudo systemctl reload apache2.service``) to reload Apache Web Server  

to proceed.

### 3) Edit Your ``/etc/hosts`` File

To view your website in your favorite web browser, just edit your ``/etc/hosts`` file and add  ``example1.localhost`` in it. After that, just create an helloworld file as your index (for example; index.html) in your website directory, open your browser and go to http://example1.localhost. You should see the helloworld page that you created.

Example ``/etc/hosts`` file;

```bash
127.0.0.1	localhost
127.0.0.1 	example1.localhost
```

Example helloworld ``index.html`` file;

```html
<html>
<header><title>This is title</title></header>
<body>
Hello world
</body>
</html>
```
And you are ready to go! Just repeat these steps to host multiple local websites with Apache.


