Step 1 — Installing Apache and Updating the Firewall

a) Install Apache using Ubuntu’s package manager ‘apt’

- [ ] sudo apt update 

![1 a i](https://user-images.githubusercontent.com/10243139/115963443-33fd4800-a517-11eb-88ce-f8d4e19d8b34.jpg)

- [ ] sudo apt install apache2

![1 a ii](https://user-images.githubusercontent.com/10243139/115963600-ea612d00-a517-11eb-8847-b060bc852ff0.jpg)

b) To verify that apache2 is running as a Service in our OS, use following command
	
- [ ] sudo systemctl status apache2

![1 b](https://user-images.githubusercontent.com/10243139/115963706-7bd09f00-a518-11eb-87e5-66abf48f01e0.jpg)

 Open inbound port 80

Go to Instances - Security Groups - Edit Inbound Rules - Add rule
	   	Add type - HTTP
Add Custom ip address block - 0.0.0.0/0

![1 c i](https://user-images.githubusercontent.com/10243139/115963761-af132e00-a518-11eb-9bd1-799cef78e2a0.jpg)

Test rule in Ubuntu Shell, run: 

- [ ] curl http://localhost:80
		
	or
	
- [ ] curl http://127.0.0.1:80
		
![1 c ii](https://user-images.githubusercontent.com/10243139/115964584-1d59ef80-a51d-11eb-9511-dfff4380e923.jpg)

d) Test Apache HTTP Server

http://<Public-IP-Address>:80
	
![1 d i](https://user-images.githubusercontent.com/10243139/115964674-82ade080-a51d-11eb-9971-8fe6d5eb4a65.jpg)


Step 2 — Installing MySQL


a) Use ‘apt’ to acquire and install mysql-server

- [ ] sudo apt install mysql-server

![2 a i](https://user-images.githubusercontent.com/10243139/115964804-0bc51780-a51e-11eb-981e-ad4896d6c34f.jpg)

Run security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system

- [ ] VALIDATE PASSWORD COMPONENT - Y
- [ ] Level of Password I choose 0 for LOW
- [ ] Enter a secure password and confirm password
- [ ] Every other step was Y or or y Yes
- [ ] Reload priviledge tables - Y
- [ ] Success. All done!

![2 a ii](https://user-images.githubusercontent.com/10243139/115964847-40d16a00-a51e-11eb-872a-9f16fec604e5.jpg)


b) Log in to the MySQL console and Exit back to Ubuntu Console

- [ ] sudo mysql	

![2 b i](https://user-images.githubusercontent.com/10243139/115966070-f652ec00-a523-11eb-8a0a-7a60f3fee847.jpg)

- [ ] exit

![2 b ii](https://user-images.githubusercontent.com/10243139/115966079-01a61780-a524-11eb-837e-f034af17f838.jpg)



Step 3 — Installing PHP

a) Install at the same time php and php-mysql packages, as well as libapache2-mod-php to enable Apache to handle PHP files
	
- [ ] sudo apt install php libapache2-mod-php php-mysql 

![3 a i](https://user-images.githubusercontent.com/10243139/115966092-12ef2400-a524-11eb-9ef8-312161cb171a.jpg)


b) Confirm PHP version

- [ ] php -v

![3 b i](https://user-images.githubusercontent.com/10243139/115966106-27cbb780-a524-11eb-8745-2cb9db3def28.jpg)



Step 4 — Creating a Virtual Host for your Website using Apache

a) Create a directory for projecttesla using mkdir command and ownership of the directory with the $USER environment variable
- [ ] sudo mkdir /var/www/projectesla 
	
- [ ] sudo chown -R $USER:$USER /var/www/projectesla 

![4 a](https://user-images.githubusercontent.com/10243139/115966285-c2c49180-a524-11eb-882a-19fd82bfb672.jpg)

b) Create and open a new configuration file in Apache's sites-available directory and add input a new configuration

- [ ] sudo nano /etc/apache2/sites-available/projectesla.conf
	
- [ ] Insert the following configuration:

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
	
![4 b ii](https://user-images.githubusercontent.com/10243139/115966333-01f2e280-a525-11eb-8e65-36eddaba6e0f.jpg)

Use the command below to show the new file in the sites-available directory

- [ ] sudo ls /etc/apache2/sites-available 

![4 b iii](https://user-images.githubusercontent.com/10243139/115966403-50a07c80-a525-11eb-8dd7-0487a724e091.jpg)

c) Enable the new virtual host

i)	Use a2ensite command to enable the new virtual host:
		- [ ] sudo a2ensite projectesla
ii)	Disable the default website that come installed with Apache:
		- [ ] sudo a2dissite 000-default
iii)	Make sure your configuration file doesn't contain syntax errors, run:
		- [ ] sudo apache2ctl configtest
iv)	Reload Apache to save changes and take effect:
		- [ ] sudo systemctl reload apache2

![4 c](https://user-images.githubusercontent.com/10243139/115966467-95c4ae80-a525-11eb-89bf-42c0bd67e397.jpg)

d) Create a html file and open on your browser

i)	Create an empty html file and and input into it with echo:
		- [ ] sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s 						http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectesla/index.html
		
ii)	Go to your browser and type the URL:
		- [ ] http://<Public-IP-Address>:80

![4 d](https://user-images.githubusercontent.com/10243139/115966514-d290a580-a525-11eb-9eea-e09c4ba6a3be.jpg)


Step 5 — Enable PHP on the website


a) Change Directory.conf file to change order to enable index.php to come before index.html
	
- [ ] sudo nano /etc/apache2/mods-enabled/dir.conf

	save and exit back to the terminal
	
	![5 a i](https://user-images.githubusercontent.com/10243139/115966547-010e8080-a526-11eb-8018-0cf56380322d.jpg)

b) Reload Apache so the changes can take effect with this command:
		
- [ ] sudo systemctl reload apache2

c) Create a new index.php file and input a valid PHP code inside:

i) create a new php file with the command:

- [ ] nano /var/www/projectesla/index.php

![5 c ii](https://user-images.githubusercontent.com/10243139/115966599-492da300-a526-11eb-84b6-a6bf68a7d047.jpg)

- [ ] Input in the file the following text:
		<?php
		phpinfo();

d) Refresh your page
		
![5 d](https://user-images.githubusercontent.com/10243139/115966604-4fbc1a80-a526-11eb-8582-59b58a7ba8c1.jpg)


