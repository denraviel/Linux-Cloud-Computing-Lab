Connecteed with instance used in previous lab :`ssh -i ProdCafeServer.pem ec2-user@44.225.208.140`

Download the latest WordPress installation package with the wget command. The following command should always download the latest release: `wget https://wordpress.org/latest.tar.gz`

Unzip and unarchive the installation package. The installation folder is unzipped to a folder called wordpress :`tar -xzf latest.tar.gz`

To create a database user and database for your WordPress installation
Start the database server:`sudo systemctl start mariadb`

Log in to the database server as the root user. Enter your database root password when prompted; this may be different than your root system password, or it might even be empty if you have not secured your database server :`mysql -u root -p`
 Entered password for secured root user

Create a user and password for your MySQL database. Your WordPress installation uses these values to communicate with your MySQL database :`CREATE USER 'SchullDenzino'@'localhost' IDENTIFIED BY 'Dbonee8d';`
 Returned output : Query OK, 0 rows affected (0.00 sec)

 Create your database. Give your database a descriptive, meaningful name : ``CREATE DATABASE `wordpress-db`;``
  Returned output : Query OK, 1 row affected (0.00 sec)

Grant full privileges for your database to the WordPress user that you created earlier :``GRANT ALL PRIVILEGES ON `wordpress-db`.* TO "SchullDenzino"@"localhost";``

Flush the database privileges to pick up all of your changes : `FLUSH PRIVILEGES;`

Exit the mysql client: `exit`

To create and edit the wp-config.php file
Copy the wp-config-sample.php file to a file called wp-config.php. This creates a new configuration file and keeps the original sample file intact as a backup:`cp wordpress/wp-config-sample.php wordpress/wp-config.php`

Edit the wp-config.php file with your favorite text editor (such as nano or vim) and enter values for your installation. If you do not have a favorite text editor :`nano wordpress/wp-config.php`

 Find the line that defines DB_NAME and change database_name_here to the database name that you created `define('DB_USER', 'wordpress-db');`

 Find the line that defines DB_USER and change username_here to the database user that you created `define('DB_USER', 'wordpress-user');`

 Find the line that defines DB_PASSWORD and change password_here to the strong password that you created `define('DB_PASSWORD', 'password');`

 To install your WordPress files under the Apache document root : `cp -r wordpress/* /var/www/html/`

To install the PHP graphics drawing library on Amazon Linux 2
`sudo yum install php-gd`

To fix file permissions for the Apache web server
`sudo chown -R apache /var/www`

Grant group ownership of /var/www and its contents to the apache group
`sudo chgrp -R apache /var/www`

Change the directory permissions of /var/www and its subdirectories to add group write permissions and to set the group ID on future subdirectories
`sudo chmod 2775 /var/www`
`find /var/www -type d -exec sudo chmod 2775 {} \;`

Recursively change the file permissions of /var/www and its subdirectories
`find /var/www -type f -exec sudo chmod 0644 {} \;`

Restart the Apache web server to pick up the new group and permissions
`sudo systemctl restart httpd`

Run the WordPress installation script with Amazon Linux 2
 Use the systemctl command to ensure that the httpd and database services start at every system boot
 `sudo systemctl enable httpd && sudo systemctl enable mariadb`

Verify that the database server is running
 `sudo systemctl start mariadb`

Verify that your Apache web server (httpd) is running
`sudo systemctl status httpd`






