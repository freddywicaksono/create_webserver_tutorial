# Create A Web Server Using Ubuntu 20.04

1. Install Ubuntu 20.04

    https://phoenixnap.com/kb/install-ubuntu-20-04

2. Install Apache
    
        sudo apt update
        
        sudo apt install apache2
        
    Check Apache Version:
    
        apache2 -version
        
    Add apache to Firewall
    
        sudo ufw allow 'Apache'
     
    Check Firewall status
    
        sudo ufw status
        
    Mengaktifkan Firewall
    
        sudo ufw enable
        
    Restart Apache service
    
        sudo service apache2 restart
        
    Test apache on Browser
    
        http://localhost    or  http://<ip server>/
        
    Source:
        
    https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04-id

3. Activate User Directory, every user will have their own directory. Example: http://<ip_server>/~user
    
        sudo apt update
        
        sudo a2enmod userdir
        
    Restart Apache
    
        sudo systemctl restart apache2
        
    Create public_html directory for current user
    
        mkdir ~/public_html
        
    Activate public_html direcory
    
        cd /home/[user name]/public_html
        
    Create a HTML file for testing purposes
    
        sudo touch index.html
        
        sudo nano index.html
        
        { please write a simple html script }
        
    Test web page 
    
        http://<ip server>/~[user name]
        
    or 
    
        http://localhost/~[user name]
        
    Add New apache User:
    
        sudo adduser [user name]
        
    Example:
    
        sudo adduser smith
        
    Switch between user:
    
        su - [user name]
        
    Check active folder:
    
        pwd
        
    Source:
        
    https://www.server-world.info/en/note?os=Ubuntu_20.04&p=httpd&f=4

4. Install PHP 7.4

        sudo apt update
        
        sudo apt install php7.4
                
        sudo apt install php7.4-common php7.4-fpm php7.4-json php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-bcmath libapache2-mod-php7.4 php7.4-intl -y

5. Activate execute/run php script on user directory

    edit file : php*.conf
    
        sudo nano /etc/apache2/mods-available/php*.conf
    
    Change : 
    
        php_admin_flag engine Off --> php_admin_flag engine On
        
    restart apache:
    
        sudo systemctl restart apache2
        
    create a simple php script for testing purposes:
    this user should have public_html directory, if not create it first 
    
    Activate public_html folder
    
        cd /home/[user name]/public_html
    
    Create file test.php
    
        sudo touch test.php
    
    Edit file test.php
    
        sudo nano test.php
    
    Example script:
    
        <?php
           echo "Welcome to PHP page";
        ?> 
        
    Test php script on user folder
    
        http://<ip server>/~[user name]/test.php
    
    
6. Install Mariadb 10.5

        sudo apt update
        
        sudo apt -y install software-properties-common
        
        sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
        
        sudo add-apt-repository 'deb [arch=amd64] http://mariadb.mirror.globo.tech/repo/10.5/ubuntu focal main'
        
        sudo apt update
        
        sudo apt install mariadb-server mariadb-client
        
        sudo mysql_secure_installation
        
    Interactive input:
    
        Enter current password for root (enter for none): <press enter>
        
	    Switch to unix_socket authentication [Y/n] y

        Change the root password? [Y/n] y
        
        New password: <fill it>
        
        Re-enter new password:<fill it> 

        Remove anonymous users? [Y/n] y
        
        Disallow root login remotely? [Y/n] n
        
        Remove test database and access to it? [Y/n] y
        
        Reload privilege tables now? [Y/n] y
        
    Test login:
    
        mysql -u root -p <press enter>
        
        password: <fill it> <press enter>
           
    https://computingforgeeks.com/how-to-install-mariadb-on-ubuntu-focal-fossa/
    
7. Remote Access Mariadb
    
   Create a special db user
    
        > create user '[username]'@'%' identified by '[user_passwd]';
    
    example:
    
    	> create user 'smith'@'%' identified by 'secretpass';
    
   Create a database for the user above:
    
        > create database [database_name];
        
     example:
    
    	> create database akademik;
    	

   Give the user to access and manage the database:
    
        > grant all privileges on [database_name].* to '[username]'@'%' identified by '[user_passwd]';
     
     example:
     
     	> grant all privileges on akademik.* to 'smith'@'%' identified by 'secretpass';
    
   Add port 3306 to firewall:
    
        sudo ufw allow 3306/tcp
   
   Reload Firewall:
    
        sudo ufw disable
    
        sudo ufw enable
    
   Edit file : 50-server.cnf
    
        sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
    
   change:

        bind-address = 127.0.0.1 --> 0.0.0.0
    
   Reload Mariadb:
    
        sudo systemctl restart mysql

8. Install PHPMyAdmin

        sudo apt update
        
        sudo apt install phpmyadmin
        
   When prompted to choose the webserver, selecat apache2 and continue.    
   
        +------------------------+ Configuring phpmyadmin +-------------------------+
        | Please choose the web server that should be automatically configured to   |
        | run phpMyAdmin.                                                           |  
        | Web server to reconfigure automatically:                                  |
        |                                                                           |
        |    [*] apache2                                                            |
        |    [ ] lighttpd                                                           |    
        |                                 <ok>                                      |
        +---------------------------------------------------------------------------+
        
   When prompted again to allow debconfig-common to install a database and configure select No.
   
        +------------------------+ Configuring phpmyadmin +-------------------------+
        |                                                                           |
        | The phpmyadmin package must have a database installed and configured      |
        | before it can be used.  This can be optionally handled with               |
        | dbconfig-common.                                                          |
        |                                                                           |
        | If you are an advanced database administrator and know that you want to   |
        | perform this configuration manually, or if your database has already      |
        | been installed and configured, you should refuse this option.  Details    |
        | on what needs to be done should most likely be provided in                |
        | /usr/share/doc/phpmyadmin.                                                |
        |                                                                           |
        | Otherwise, you should probably choose this option.                        |
        |                                                                           |
        | Configure database for phpmyadmin with dbconfig-common?                   |
        |                                                                           |
        |                  <Yes>                  <No>                              |
        |                                                                           |
        +---------------------------------------------------------------------------+
        
  After installing, run the commands below to logon to the database server to enable phpMyAdmin root logon.

  Now, open your web browser and login to the server hostname or IP address followed by phpmyadmin

        http://localhost/phpmyadmin
	
  or
    
        http://<ip server>/phpmyadmin
