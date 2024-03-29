# Wordpress-5-installation-runbook
Complete guide to installing wordpress 5 on Ubuntu 16.04 server.
# Introduction 
A complete guide to installing wordpress on your Ubuntu 16.04 server via command line. 
Includes installing the LAMP stack - Linux, Apache, MySql, and PHP needed to install wordpress.
## Requirements 
Ubuntu 16.04 server installed and configured with sudo (administrator privileges).
## It's as easy as copying and pasting each of the commands.
# Step 1 - Install Apache2 
Update the Ubuntu package manager
```
sudo apt-get update
```
* Enter your sudo (admin password) when prompted.
#### Install Apache
```
sudo apt-get install apache2
```
* Enter 'Y' to agree to disk space requirements.
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
Enter MySQL root user password (from the step above)
#### Answer questions about securing MySQL settings
1. Configuring validate password plugin - no (answering yes can create errors down the road)
2. Change the password for root? no (but it's up to you)
3. Remove anonymous users? yes 
4. Disable remote login? yes
5. Remove test database? yes
6. Reload privilege tables now? yes
# Step 3 - Install PHP 
Install PHP 7.0
```
sudo apt-get install php7.0 libapache2-mod-php7.0 php7.0-mysql php7.0-curl php7.0-mbstring php7.0-gd php7.0-xml php7.0-xmlrpc php7.0-intl php7.0-soap php7.0-zip 
```
* Enter Y to agree to disk space requirements.
### we can test that PHP is installed correctly by replacing the index.html file with an index.php file 
Navigate to the HTML directory
```
cd /var/www/html/
```
Create and edit a new php file
```
sudo nano index.php
```
Copy and paste this php script into the open editor.
```
<?php phpinfo(); ?>
```
* Press <kbd>Ctrl-o</kbd> to save your changes.
* Press <kbd>Enter</kbd>
* Press <kbd>Ctrl-x</kbd> to exit the editor.
#### Delete the index.html file.
```
sudo rm index.html
```
Restart Apache to be sure your changes have taken effect.
```
sudo systemctl restart apache2
```
### Now when you enter your server's IP adress in your browser you should see The PHP page with the version (7.0) that your server is running. 
# Step 4 - Now you're ready to install wordpress
You should still be in the HTML directory /var/www/html/
#### We will download The latest version of WordPress
```
sudo wget -c http://wordpress.org/latest.tar.gz
```
Extract file into /wordpress/ directory (it creates the wordpress directory when it extracts the file)
```
sudo tar -xzvf latest.tar.gz
```
Set permissions for the wordpress directory you just created.
```
sudo chown -R www-data:www-data /var/www/html/wordpress
```
# Step 6 - Create a database in MySQL for WordPress
Login to MySQL
```
mysql -u root -p
```
* Enter the MySQL root password created in step 3 above.
#### Create a new MySQL database for WordPress installation.
* wordpress_db is the name of your database (can be anything).
```
CREATE DATABASE wordpress_db;
```
* wordpress_user is a new user_name (your choice) and PASSWORD is the password for the user (make it strong and unique).
```
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'PASSWORD';
```
Enable the changes to the new user.
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
Rename the the sample configuration file.
```
sudo mv wp-config-sample.php wp-config.php
```
Open and edit the wp-config file.
```
sudo nano wp-config.php
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
* Press <kbd>Enter</kbd>
* Press <kbd>Ctrl-x</kbd> to exit the editor.
Restart Apache and MySQL
```
systemctl restart apache2
```
* Enter your sudo (admin) password
```
systemctl restart mysql
```
* Enter your sudo (admin) password
## You're all done. You can go to your server's IP address in a browser of your choice and follow the on screen instructions to finish the installation. 
```
http://your_server_ip_address/wordpress
```


