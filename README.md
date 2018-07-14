# Wordpress stack for ARM devices

## What is this?
This repo contains a Dockerized Wordpress stack based on the following components;
- MariaDB (Database backend, it a replacement for MySQL)
- PHP/Wordpress (the webserver hosting Wordpress itself)
- NGINX (publish the webserver to the outside world)


## Storage requirements
Because some files are shared between the wordpress and nginx container, it's required to have a shared directory for both containers.
For testing this could be done on for example your notebook/desktop but even better if you have a NAS or NFS server running in the network is to store the files on a NAS.

### Directory structure;
In the example commands shown here below the directory structure is created in the home folder.
The prefer way is to store this somewhere else, but for testing this is fine.
- mariadb [/home/[user]/shared/mariadb
- wordpress [/home/[user]/shared/mariadb
- nginx [/home/[user]/shared/nginx

### Running the database;

docker run \
  --detach \
  --name mariadb \
  --volume /home/[user]/shared/mariadb:/var/lib/mysql \
  --env MYSQL_ROOT_PASSWORD="rootpassword123" \
  --env MYSQL_DATABASE="wordpress" \
  --env MYSQL_USER="wordpress" \
  --env MYSQL_PASSWORD="password123" \
  --publish 3306 \
  remonlam/docker-wordpress-arm:mariadb-armhf


### Running the webserver;

docker run \
  --detach \
  --name wordpress \
  --link mariadb \
  --volume /home/[user]/shared/wordpress:/var/www/html \
  --env WORDPRESS_TABLE_PREFIX="wp_" \
  --env WORDPRESS_DB_HOST="mariadb" \
  --env WORDPRESS_DB_NAME="wordpress" \
  --env WORDPRESS_DB_USER="wordpress" \
  --env WORDPRESS_DB_PASSWORD="password123" \
  --publish 9000:9000 \
  remonlam/docker-wordpress-arm:wordpress-armhf


### Running nginx proxy;

docker run \
  --detach \
  --name nginx \
  --link wordpress \
  --volume /home/[user]/shared/nginx:/etc/nginx/conf.d \
  --volume /home/[user]/shared/wordpress:/var/www/html \
  --publish 80:80 \
  remonlam/docker-wordpress-arm:nginx-armhf
