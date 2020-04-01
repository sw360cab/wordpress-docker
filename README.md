# Dockerized Wordpress

This project provides docker compose services configuration for:

* Wordpress CMS (on top of Nginx and based on [webdevops/php-nginx:alpine-php7](https://dockerfile.readthedocs.io/en/latest/content/DockerImages/dockerfiles/php-nginx.html) image)
* MariaDB (as MySQL databse behind WP)

## Initial Configuration

1. Download and unzip [Wordpress](https://wordpress.org/latest.zip) (or any backuped version of it) into **${project}/data/wp**.
2. Update th Env File (MariaDB env file) at **${project}/mariadb.env**, with the following data:

       MYSQL_ROOT_PASSWORD=<dbms_root_pwd>
       MYSQL_DATABASE=<db_name>
       MYSQL_USER=<db_user>
       MYSQL_PASSWORD=<db_password>
3. (if any) Restore Maria DB databases (either by coping files into **${project}/data/db** or  by launching MySQL client when starting from a backup created with *mysqldump*.

   e.g. (supposing that a dump name *dump.sql* is placed into *${project}/data/db*)

       docker-compose run db mysql -uroot -p $MYSQL_ROOT_PASSWORD /var/lib/mysql/dump.sql
4. Verify and report MariaDB configuration (env file) into Wordpress config **${project}/data/wp/wp-config.php**. In particular verify MySQL db hostname should be:

       define( 'DB_HOST', 'db' );
5. Uncomment and modify server name in **${project}/wordpress/wp_vhost.conf**, setting the one of your virtual server:

       server_name <your_dns_hostname>
6. (opt.) Add SLL/TLS certificates into Nginx, mapping files onto the following container paths:
    * Certificate - **/opt/docker/etc/nginx/ssl/server.crt**
    * Private Key - **/opt/docker/etc/nginx/ssl/server.key**

## Run and enjoy

You are now ready to start your own Wordpress CMS:

    docker-compose up -d

* Db data will be stored **${project}/data/db**
* Wordpress data will be stored at **${project}/data/wp**

## References

* [Reference Doc for webdevops/php-nginx](https://dockerfile.readthedocs.io/en/latest/content/DockerImages/dockerfiles/php-nginx.html)
