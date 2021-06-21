![npm package](https://img.shields.io/badge/nginx-1.18.0-brightgreen.svg)
![npm package](https://img.shields.io/badge/php_fpm-7.4.3-blue.svg)
![npm package](https://img.shields.io/badge/mysql-8.0.25-orange.svg)

<h1>Introduction</h1>
The aim of this repository is to show how to set up servers (one for nginx(front-end) and another one for mysql(back-end)) and configure them in order to be able to work together.
The infrastructure built in cloud provided by Amazon Web Services.

<h1>Pre-requisites</h1>

- need to have an AWS account
- need to be able to use bash shell confidently
- must know how to config an nginx virtualhost
- basic knowledge of mysql (Structured Query Language)

<h2>Create servers</h2>

Create 2 servers on EC2, choose ubuntu 20.04LTS. Once the servers installed, connect to via ssh connection. (*The key must be installed on host computer at the last stage of installation process*)

Because this is a newly installed server, we need to update the packages. After that, install nginx.
For testing purposes, warmly recommended to use a self-signed certificate for using SSL connection rather than http in URL.

Don't forget to modify the file permissions of .csr, .key and .crt!

Create virtual hosts (*sudo vi /etc/nginx/sites-avaialble/virthost-1*) to test how it can be reached the different contents via url, just like if we would have used more servers.
To do so, the most easiest way is just copy the default configfile and modify it (*server_name, root...*), uncomment line if necessary.
Don't forget to reboot the nginx to update the modifications.

<h3>Make the server more interactive...</h3>
Once we have done the previous steps, we also need to install some php modules with the php itself.
The aim of doing that is to make connection with the mysql server behind the scenes, later on... :)

For testing how the php works, let's create our first php file:

sudo vi info.php --> php phpinfo(); ?> --> Once we created, reboot the nginx and typing on the URL: https://our-servername/info.php

We should see a nice blue coloured lines. (*those are parameters of the php BTW*)
Once we make sure the php working properly, delete this file immediately, because it contents sensitive datas of our server.

<h2>Let's jump into the mysql server</h2>

We only need to install the mysql server. After that, warmly recommended to make some security settings using of mysql_secure_installation:

$ sudo mysql_secure_installation

For example, create a root password (*which is not the same in mysql as in the server environment*), remove anonymous users, disallow root login remotely, etc.

Once we done that, let's do some nice things in mysql:

#![Image of mysql](https://github-pictures.s3.amazonaws.com/mysql.png)

In order to be reachable our newly configured mysql server, we need a bit to modify the main mysql config:

$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

change the bind-address to 0.0.0.0

$ sudo systemctl restart mysql

That is it, nearly there. :)
