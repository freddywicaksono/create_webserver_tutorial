1. Install Ubuntu 20.04

    https://phoenixnap.com/kb/install-ubuntu-20-04

2. Install Apache

    https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04-id

3. Activate User Directory, every user will have their own directory. Example: http://<ip_server>/~user

    https://www.server-world.info/en/note?os=Ubuntu_20.04&p=httpd&f=4

4. Activate execute/run php script on user directory

    edit file : php*.conf
    
        sudo nano /etc/apache2/mods-available/php*.conf
    
    Change : 
    
        php_admin_flag engine Off --> php_admin_flag engine On
    Save: Crtl-O <enter>
    
    Exit: Ctrl-X
    
    restart apache:
    
        sudo systemctl restart apache2
    
