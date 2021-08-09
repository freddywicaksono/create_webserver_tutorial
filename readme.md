1. Install Ubuntu 20.04

    https://phoenixnap.com/kb/install-ubuntu-20-04

2. Install Apache

    https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04-id

3. Activate User Directory, every user will have their own directory. Example: http://<ip_server>/~user

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
    
6. Install Mariadb 10.5
    
    https://computingforgeeks.com/how-to-install-mariadb-on-ubuntu-focal-fossa/
    
7. Remote Access Mariadb
    
   Create a special db user
    
        > create user '[username]'@'%' identified by '[user_passwd]';
    
   Create a database for the user above:
    
        > create database '[database_name]';
    
   Give the user to access and manage the database:
    
        > grant all privileges on [database_name].* to '[username]'@'%' identified by '[user_passwd]';
    
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
