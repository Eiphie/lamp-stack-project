# LAMP stack
LAMP is an acronym for Linux, Apache, MySQL, PHP or Python or Perl, which is a set of technology framework and tools used to develop a software product.
## Project Overview
This is a simple project demonstrating the setup of LAMP stack comprising Linux, Apache, MySQL, PHP, and deployed on AWS EC2 instance.
## Project SetUp
This project assumes that you have an active AWS account. Thus the steps of creating an AWS account is skipped.
### Launch EC2 instance:
- On the AWS console, use the search bar to search for EC2, click on the EC2 search result.
- On the EC2 dashboard, click on `Launch instance`
- Enter required information to Launch an Ubuntu 20.04 LTS.
- Ensure that you create a Key pair. This will be used to `ssh` into the instance.
- Create a security group that allows inbound rule on port 22. Set your IP address as the source.
- Specify volume size. 8GiB should be enough.
- Launch instance.
<img width="1096" height="779" alt="Screenshot 2025-08-11 at 21 49 32" src="https://github.com/user-attachments/assets/db743ec7-33c4-46ea-bc06-4fc8e368ea1d" />


### Installations:
- ssh into the instance
```
ssh -i <keypair.pem> ubuntu@<IPaddress>
```

- install Apache
```
sudo apt update
sudo apt install apache2
```
At this point Apache server is up and running. Copy your EC2 instance public IP and open in a browser. You should see the Apache landing page below.
<img width="1490" height="876" alt="Screenshot 2025-08-05 at 20 58 07" src="https://github.com/user-attachments/assets/74581a5c-4306-4006-b8b3-7cc11e95e92e" />

- Install MySQL
```
sudo apt install mysql-server
```

Verify MySQL installation.
```
sudo mysql
```

- Install PHP
```
sudo apt install php libapache2-mod-php php-mysql
```
Verify PHP installation
```
php -v
```

### Enable PHP website.
Create a ProjectLAMP directory
```
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```
Create a `projectlamp.conf` file
```
sudo vim /etc/apache2/sites-available/projectlamp.conf  
```
Populat file with snippet below;
```
<VirtualHost *:80>
ServerName projectlamp ServerAlias www.projectlamp
ServerAdmin webmaster@localhost
DocumentRoot /var/www/projectlamp
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined 
</VirtualHost>
```
Enable new virtual and disable default
```
sudo a2ensite projectlamp
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```
Use commamd below to create a `dir.conf`
```
sudo vim /etc/apache2/mod-enabled/dir.conf
```
Populate the `dir.conf` file with the snippet below;
```
<IfModule mod_dir.c>

#Change this:

#DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm

#To this:
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

</IfModule>
```
Reload Apache.
```
sudo systemctl reload apache2
```

Create `index.php` file
```
vim /var/www/projectlamp/index.php
```
Reload website on the browser. This time it should show a PHP server information.
<img width="1366" height="883" alt="Screenshot 2025-08-08 at 21 52 54" src="https://github.com/user-attachments/assets/b62d7fed-c998-4640-bca5-d5233fe3a825" />



