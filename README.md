# Wordpress-5-installation-runbook
Complete guide to installing wordpress 5 on Ubuntu 16.04 server.
# Introduction 
A complete guide to installing wordpress on your Ubuntu 16.04 server via command line. 
Includes installing the LAMP stack - Linux, Apache, MySql, and PHP needed to install wordpress.
## Requirements 
Ubuntu 16.04 server installed and configured with sudo (administrator privileges).

# Step 1 - Install Apache2 
Update the Ubuntu package manager
```
sudo apt-get update
```
Install Apache
```
sudo apt-get install apache2
```
* Enter password when prompted.
* Enter Y to agree to disk space requirements.
* You can ignore the warning about Global name server.
### Test that apache is up and running by entering your server's IP address in a browser and you should see the Ubuntu Apache2 Default page ###
# Step 2 - Install MySQL #
Install MySQL version 5.7
```
sudo apt-get install mysql-server
```
* Enter Y to agree to disk space requirements.
* Choose a password for MySQL. Make it strong and unique.
* Re-enter password to confirm.
#### Secure MySQL
```
mysql_secure_installation
```
Answer questions about secury of MySQL
1. configuring validate password plugin - no (answering yes can create errors down the road)
2. disable remote login - yes
3. remove test database - yes
# Step 3 - Install PHP 
Install PHP 7.2
```
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-sqlite php7.2-curl php7.2-intl php7.2-mbstring php7.2-xmlrpc php7.2-mysql php7.2-gd php7.2-xml php7.2-cli php7.2-zip
```
### we can test that PHP is installed correctly by replacing the index.html file with an info.php file 
Navigate to the HTML directory
```
cd /var/www/html/
```
Create and edit a new php file
```
sudo nano info.php
```
Copy and paste this php script into the open editor.
```
<?php phpinfo(); ?>
```
* Press <kbd>Ctrl-o</kbd> to save your changes.
* Press <kbd>Ctrl-x</kbd> to exit the editor.
#### Delete the index.html file.
```
sudo rm index.html
```
Restart Apache just to be sure your changes have taken effect.
```
sudo systemctl restart apache2
```
### Now when you enter your server's IP adress in your browser you should see The PHP page with the version that your server is running (7.2) 
# Step 4 - Now you're ready to install wordpress
We should still be in the HTML directory /var/www/html/
#### We will download The latest version of WordPress
```
wget -c http://wordpress.org/latest.tar.gz
```
Extract file into /wordpress/ directory
```
tar -xzvf latest.tar.gz
```
Set permissions for the wordpress directory you just created.
```
chown -R www-data:www-data /var/www/html/wordpress
```
# Step 6 - Create a database in MySQL for WordPress
Login to MySQL
```
mysql -u root -p
```
* Enter your MySQL root password created in step 3 above.
#### Create a new MySQL database for WordPress installation.
* wordpress_db is the name of your database (can be anything).
```
CREATE DATABASE wordpress_db;
```
wordpress_user is a new user_name (your choice) and PASSWORD is the password for the user (make it strong and unique).
```
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'PASSWORD';
```
Make the changes to the new user take effect.
```
FLUSH PRIVILEGES;
```
Exit MySQL
```
exit;
```
Move into the wordpress directory /var/www/html/wordpress
```
cd wordpress
```
rename the the sample configuration file
```
mv wp-config-sample.php wp-config.php
```
Open and edit the wp-cofig file.
```
nano wp-config.php
```
Update the file with your database information from the beggining of this step (step 6)
```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress_db');

/** MySQL database username */
define('DB_USER', 'wordpress_user');

/** MySQL database password */
define('DB_PASSWORD', 'PASSWORD');
```
* Press <kbd>Ctrl-o</kbd> to save your changes.
* Press <kbd>Ctrl-x</kbd> to exit the editor.
Restart Apache and MySQL
```
systemctl restart apache2
```
```
systemctl restart mysql
```
## You're all done, you can go to your server's IP adress in the browser of your choice and follow the on screen instructions to finish the installation. __http://your_server_ip_address/wordpress__


