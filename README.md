# Wordpress-5-installation-runbook
Complete guide to installing wordpress 5 on Ubuntu 16.04 server.
# Introduction #
A complete guide to installing wordpress on your Ubuntu 16.04 server via command line. 
Includes installing the LAMP stack - Linux, Apache, MySql, and PHP needed to install wordpress.
## Requirements ##
Ubuntu 16.04 server installed and configured with sudo (administrator privlages).

# Step 1 - Install Apache2 #
Update the Ubuntu pakage manager
`sudo apt-get update`
Install Apache
`sudo apt-get install apache2`
Enter password when prompted.
Enter Y to agree to disk space requirements.
You can ignore the warning about Global name server.
### test that apache is up and running by entering your server's ip address in a browser and you should see the Ubuntu Apache2 Default page ###
# Step 2 - Install MySQL #
Install MySQL version 5.7
`sudo apt-get install mysql-server`
Enter Y to agree to disk space requirements.
Choose a password for MySQL. Make it strong and unique.
Re-enter password to confirm.
Secure MySQL
`mysql_secure_installation`
Answer questions about secury of MySQL
1. configuring validate password plugin - no (answering yes can create errors down the road)
2. disable remote login - yes
3. remove test database - yes
# Step 3 - Install PHP #
Install PHP 7.2
`sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-sqlite php7.2-curl php7.2-intl php7.2-mbstring php7.2-xmlrpc php7.2-mysql php7.2-gd php7.2-xml php7.2-cli php7.2-zip`
### we can test that PHP is installed correctly by replacing the index.html file with an index.php file ###
Navigate to the HTML directory
`cd /var/www/html/`
Create and edit a new php file
`sudo nano info.php`
Copy and paste this php script into the open editor.
`<?php phpinfo(); ?>`
Press <kbd>^o</kbd> to save your changes.
Press <kbd>^x</kbd> to exit the editor.
Delete the index.html file.
`sudo rm index.html`
Restart Apache just to be sure your changes have taken effect.
`sudo systemctl restart apache2`
### Now when you enter your server's IP adress in your browser you should see The PHP page with the version that your server is running (7.2) ###



