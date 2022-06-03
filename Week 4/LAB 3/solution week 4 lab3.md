Created a Vm on aws with command : `aws ec2 run-instances --image-id ami-06eecef118bbf9259 --instance-type t2.micro --subnet-id subnet-02291a5ec55ccb635 --count 1 --security-group-ids sg-0fbdae7cca1255b9e --key-name tab2`

connected to the vm using command:`ssh -i tab2.pem ec2-user@18.234.193.25`

Updated server `sudo yum update -y`
1. Prepare the LAMP server
I can install the lamp-mariadb10.2-php7.2 and php7.2 Amazon Linux Extras repositories to get the latest versions of the LAMP MariaDB and PHP packages for Amazon Linux 2:`sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2`
Now that your instance is current, I installed the Apache web server, MariaDB, and PHP software packages.

Using the yum install command to install multiple software packages and all related dependencies at the same time :`sudo yum install -y httpd mariadb-server`
I started the Apache web server with command:`sudo systemctl start httpd`
To enable Apache web server I used command:`sudo systemctl enable httpd`
To verify that httpd is on i ran the following command:`sudo systemctl is-enabled httpd`
To set file permissions
Add your user (in this case, ec2-user) to the apache group.
`sudo usermod -a -G apache ec2-user`

To verify your membership in the apache group, reconnect to your instance, and then run the following command
`groups`

Change the group ownership of /var/www and its contents to the apache group. `sudo chown -R ec2-user:apache /var/www`

To add group write permissions and to set the group ID on future subdirectories, change the directory permissions of /var/www and its subdirectories.
:`sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;`

To add group write permissions, recursively change the file permissions of /var/www and its subdirectories:`find /var/www -type f -exec sudo chmod 0664 {} \;`


2. Test your LAMP server
 To test LAMP server
 Create a PHP file in the Apache document root:`echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php`
 In  web browser,I typed the URL of the file that I just created. This URL is the public DNS address of the instance followed by a forward slash and the file name:`44.225.208.140/phpinfo.php`

 To verify that all of the required packages were installed with the following command:`sudo yum list installed httpd mariadb-server php-mysqlnd`

 3. Secure the database server
  To secure the MariaDB server
  Start the MariaDB server :`sudo systemctl start mariadb`
  After starting MariaDB server
  Run mysql_secure_installation:`sudo mysql_secure_installation`
  It prompted to type in a password.By default, the root account does not have a password set so Pressed Enter
  
  Returned output : Setting the root password ensures that nobody can log into the MariaDB
  root user without the proper authorisation. So i pressed 'Y' to set root password, Returned output shows success after setting password

  Typed Y to remove the anonymous user accounts: Returned output : ... Success!
  Normally, root should only be allowed to connect from 'localhost'.  This ensures that someone cannot guess at the root password from the network.

  Typed Y to disable the remote root login: Returned output :  ... Success! By default MariaDB comes with a database named 'test' that anyone can access.  This is also intended only for testing, and should be removed before moving into a production environment.

  Typed Y to remove the test database: Returned output :  - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success! Reloading the privilege tables will ensure that all changes made so far will take effect immediately.

  Typed Y to reload the privilege tables and save your changes: Returned output : ... Success! Cleaning up... All done!  If you've completed all of the above steps, your MariaDB installation should now be secure. Thanks for using MariaDB!

  4. Install phpMyAdmin
   Install the required dependencies :`sudo yum install php-mbstring php-xml -y`
   Returned output : Packages successfully installed 

   Restart Apache :`sudo systemctl restart httpd`
   Restart php-fpm :`sudo systemctl restart php-fpm`
   Navigate to the Apache document root at /var/www/html :`cd /var/www/html`
Select a source package for the latest phpMyAdmin release. To download the file directly to your instance, copy the link and paste it into a wget command : `wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz`

Create a phpMyAdmin folder and extract the package into it with the following command :`mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1`

Delete the phpMyAdmin-latest-all-languages.tar.gz tarball :`rm phpMyAdmin-latest-all-languages.tar.gz`

Starting MySql server :`sudo systemctl start mariadb`

In a web browser, I typed the URL of my phpMyAdmin installation. This URL is the public DNS address (or the public IP address) of your instance followed by a forward slash and the name of your installation directory :`44.225.208.140/phpMyAdmin`


