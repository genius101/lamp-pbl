Step 1 — Installing Apache and Updating the Firewall

a) Install Apache using Ubuntu’s package manager ‘apt’

- [x] sudo apt update 

![1 a i](https://user-images.githubusercontent.com/10243139/115963443-33fd4800-a517-11eb-88ce-f8d4e19d8b34.jpg)

- [x] sudo apt install apache2

![1 a ii](https://user-images.githubusercontent.com/10243139/115963600-ea612d00-a517-11eb-8847-b060bc852ff0.jpg)


b) To verify that apache2 is running as a Service in our OS, use following command
	sudo systemctl status apache2

c) Open inbound port 80
	i)	Go to Instances - Security Groups - Edit Inbound Rules - Add rule
	   	Add type - HTTP
Add Custom ip address block - 0.0.0.0/0
	ii)	Test rule in Ubuntu Shell, run: 
		curl http://localhost:80
			or
		curl http://127.0.0.1:80

d) Test Apache HTTP Server
	i)	http://<Public-IP-Address>:80


Step 2 — Installing MySQL

a) Use ‘apt’ to acquire and install mysql-server
	i)	sudo apt install mysql-server
	ii)	Run security script that comes pre-installed with MySQL. This script will remove some insecure default settings and 		lock down access to your database system
		VALIDATE PASSWORD COMPONENT - Y
		Level of Password I choose 0 for LOW
		Enter a secure password and confirm password
		Every other step was Y or or y Yes
		Reload priviledge tables - Y
		Success. All done!

b) Log in to the MySQL console and Exit back to Ubuntu Console
	i)	sudo mysql	
	ii)	exit


Step 3 — Installing PHP

a) Install at the same time php and php-mysql packages, as well as libapache2-mod-php to enable Apache to handle PHP files
	i)	sudo apt install php libapache2-mod-php php-mysql 

b) Confirm PHP version
	i)	php -v


Step 4 — Creating a Virtual Host for your Website using Apache

a) Create a directory for projecttesla using mkdir command and ownership of the directory with the $USER environment variable
	i)	sudo mkdir /var/www/projectesla 
	ii)	sudo chown -R $USER:$USER /var/www/projectesla 

b) Create and open a new configuration file in Apache's sites-available directory and add input a new configuration
	i)	sudo nano /etc/apache2/sites-available/projectesla.conf
	ii)	Insert the following configuration:
		<VirtualHost *:80>
    			ServerName projectesla
    			ServerAlias www.projectesla 
    			ServerAdmin webmaster@localhost
    			DocumentRoot /var/www/projectesla
    			ErrorLog ${APACHE_LOG_DIR}/error.log
    			CustomLog ${APACHE_LOG_DIR}/access.log combined
		</VirtualHost>
		Hit ctrl x to exit
		Type y to save
		Hit enter to save and exit back to the Ubuntu console
	iii)	use the command sudo ls /etc/apache2/sites-available to show the new file in the sites-available directory

c) Enable the new virtual host
	i)	Use a2ensite command to enable the new virtual host:
		sudo a2ensite projectesla
	ii)	Disable the default website that come installed with Apache:
		sudo a2dissite 000-default
	iii)	Make sure your configuration file doesn't contain syntax errors, run:
		sudo apache2ctl configtest
	iv)	Reload Apache to save changes and take effect:
		sudo systemctl reload apache2

d) Create a html file and open on your browser
	i)	Create an empty html file and and input into it with echo:
		sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public 		IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectesla/index.html
	ii)	Go to your browser and type the URL:
		http://<Public-IP-Address>:80


Step 5 — Enable PHP on the website

a) Change Directory.conf file to change order to enable index.php to come before index.html
	i)	sudo nano /etc/apache2/mods-enabled/dir.conf
		save and exit back to the terminal

b) Reload Apache so the changes can take effect with this command:
		sudo systemctl reload apache2

c) Create a new index.php file and input a valid PHP code inside:
	i)	create a new php file with the command: nano /var/www/projectesla/index.php
	ii)	Input in the file the following text:
		<?php
		phpinfo();

d) Refresh your page
		


