# WordPress_Mariadb_Nginx
## WordPress website hosting with Nginx reverse proxy in RHEL instance.

### Installing *Mariadb* in RHEL**

1. `sudo dnf install mariadb mariadb-server` : use this command to download Mariadb
2. `systemctl enable --now mariadb` : to enable and start the service of mariadb
3. `mysql_secure_installation` : to start the installation process
    
    ![mariadb installation.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/e626136d5cd994af9115d9e4e497d70267f0b31b/mariadb%20installation.png)
    
4. `mysql -u root` : to enter the mariadb shell
5. Then create a database, user and it’s password.
    
    ![creating mariadb database.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/creating%20mariadb%20database.png)
    
    `FLUSH PRIVILEDGES;` is used to reload the privilege tables.
    

### Installing the *WordPress* and it’s configuration.

1. `dnf install php php-mysqlnd php-fpm` : install the php and mysql extensions
    
    ![php installation.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/php%20installation.png)
    
2. `wget https://wordpress.org/latest.tar.gz` : to download the latest wordpress tar file
3. `tar -xvf latest.tar.gz` : to unzip the tar file
4. `cp wordpress/wp-config-sample.php wordpress/wp-config.php` : to make a config file 
5. `vim wordpress/wp-config.php` : to enter the file and edit 
    
    ![add database.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/add%20database.png)
    
    Add the DB_NAME, DB_USER, DB_PASSWORD, DB_HOST in the adjacent value.
    

### Installing and configuring *HTTP* server

1. `dnf install httpd` : to install http server
2. `vim /etc/httpd/conf/httpd.conf` : to enter the config file to httpd and change the port to 8080
    
    ```bash
    listen 8080
    ```
    
3. `cp wordpress/ /var/www/html/` : to copy the wordpress directory in the document root directory
4. `vim /etc/httpd/conf.d/vhost.conf` : create a virtual host config file and add
    
    ![virtual hosting.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/virtual%20hosting.png)
    
5. `systemctl enable --now httpd` : to enable and start httpd service

### Installing and configuring ***Nginx* server

1. `dnf install nginx` : to install nginx server
2. `vim /etc/nginx/nginx.conf` : add the proxy configuration in the main configuration file
    
    ![Nginx configuration.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/nginx%20configuration.png)
    
3. `systemctl enable --now nginx` : to enable and start the nginx server

> In RHEL instances it’s required to add the ports 80,8080 and http service in the firewalld using `firewall-cmd --add-service={}` and `firewall-cmd --add-port={}`
> 

> It’s also required to on the boolean of `http_can_network_connect` using `semanage boolean -m --on http_can_network_connect`
> 

### Access the wordpress website from browser

1. Search for `http://ip_address` in the browser and this page will appear
    
    ![web installation.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/web%20installation.png)
    
    Enter all the required values and press install WordPress
    
2. The sample web page will appear.
    
    ![website.png](https://github.com/Siddhant00Tiwari/WordPress_Mariadb_Nginx/blob/f8446f9b950183c03bece2478dbe354bcb9fc6ac/website.png)
