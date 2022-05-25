# Project 1
## Lamp Stack Implementation

Updated list of packages in package manager using 
`sudo apt update`
![alt text](./images/1.%20sudo%20apt%20update.png)

---
Ran apache2 package installation using th command
`sudo apt install apt apache2`
![alt text](./images/2%20sudo%20apt%20install%20apache2.png)

---
Verified that apache2 is running as a Service in my OS, using the command
`sudo systemctl status apache2`
![alt text](./images/3%20sudo%20systemctl%20status%20apache2.png)

---
Added a rule to EC2 configuration to open inbound connection through port 80, inorder to access it via SSH

Using the command below to access it locally in our ubuntu shell
`curl http://127.0.0.1:80`
![alt text](./images/4%20curl%20http%20127.0.0.1%2080.png)

Also to confirm it can responds to requests from the internet I run http://my-ip-address:80 on my browser
![alt text](./images/6%20confirmation%20of%20apache%20on%20browser.png)

---
To install the DBMS (mysql), this apt command was ran
`sudo apt install mysql-server`
![alt text](./images/7%20sudo%20apt%20install%20mysql-server.png)

To access mysql using
`sudo mysql`
![alt text](./images/8%20sudo%20mysql.png)

To remove insecure default setting run the security script
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';` 
![alt text](./images/9%20sql%20security.png)

Began the interactive script by running the command
`sudo mysql_secure_installation`
![alt text](./images/10%20%20validate%20password%20plugin.png)

---

Install the relevant php packages together using the command
`sudo apt install php libapache2-mod-php php-mysql`
![alt text](./images/11%20sudo%20apt%20install%20php%20libapache2.png)

To confirm php version, run:
`php -v`
![alt text](./images/12%20php%20confirmation.png)

---

Create the directory for lampproject using ‘mkdir’ command as follows:
`sudo mkdir /var/www/lampproject`

Assigning ownership of the directory with my current system user:
`sudo chown -R $BG:$BG /var/www/lampproject`

using vi or vim, I create and open a new configuration file in Apache’s sites-available directory with the command
`sudo vi /etc/apache2/sites-available/lampproject.conf`

Then I inserted the following configuration to the file
![alt text](./images/15%20vi%20code.png)

using the ls command to show the new file in the sites-available directory
`sudo ls /etc/apache2/sites-available`
It return the output below
![alt text](./images/14%20sudo%20ls%20apache2.png)

Using the command to enable the new virtual host
`sudo a2ensite lampproject`
![alt text](./images/16a%20sudo%20a2ensite.png)

To disable the default wbsite that comes with apache
`sudo a2dissite 000-default`
![alt text](./images/16%20sudo%20a2dissite.png)

To make sure the configuration file doesn’t contain syntax errors, I ran:
`sudo apache2ctl configtest`
![alt text](./images/17%20sudo%20apache2ctl%20for%20syntax%20err.png)



Finally, reloading Apache so these changes take effect:
`sudo systemctl reload apache2`

---
The new website is now active, but the web root /var/www/lampproject is still empty. I created an index.html file in that location so that that the virtual host can checked if working as expected.
Using the command below:

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/lampproject/index.html`

---

Also I ran http://my-ip-address:80 on my browser to check if the 'echo' command text shows on the browser
![alt text](./images/18%20confirmation%20apache%20virtual%20host.png)

To make index.php takes precedence over index.html, I edited /etc/apache2/mods-enabled/dir.conf.

- First by running the command:

`sudo vim /etc/apache2/mods-enabled/dir.conf`

- then editing the default file to the code below

`<IfModule mod_dir.c> DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm </IfModule>`

- Next, reload apache to effect the changes using 
`sudo systemctl reload apache2`

Create a new file named index.php inside the custom web root folder:

`vim /var/www/projectlamp/index.php`

Editing the php file with the php code below

`<?php phpinfo();`

Refreshing the opened http://my-ip-address:80 page on the browser the page bellow is shown which gives information about the server from php perspective 
![alt text](./images/19.%20php%20status%20page.png)

To protect the relevant information shown, it is best to remove the index.php file from the server. I used the 'rm' command bellow to do it.

`sudo rm /var/www/projectlamp/index.php`








