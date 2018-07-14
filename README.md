# Wordpress stack for ARM devices

## What is this?
This repo contains a Dockerized Wordpress stack based on the following components;
- MariaDB (Database backend, it a replacement for MySQL)
- PHP/Wordpress (the webserver hosting Wordpress itself)
- NGINX (publish the webserver to the outside world)


## Storage requirements
Because some files are shared between the wordpress and nginx container, it's required to have a shared directory for both containers. <br>
For testing this could be done on for example your notebook/desktop but even better if you have a NAS or NFS server running in the network is to store the files on a NAS.

### Directory structure;
In the example commands shown here below the directory structure is created in the home folder. <br>
The prefer way is to store this somewhere else, but for testing this is fine.
- mariadb [/home/[user]/shared/mariadb
- wordpress [/home/[user]/shared/mariadb
- nginx [/home/[user]/shared/nginx


## Sources
This repo contains all the code to build the containers yourself, however it could save some time to use the prebuild images from the [Docker Hub](https://hub.docker.com/r/remonlam/docker-wordpress-arm/). <br>
NOTE: it could take up to 20 minutes to build the php image.

### Image problems;
Building some images on the Raspberry Pi could take even longer than 30 minutes and may end up with out of memory errors. <br>
This happens because the system is running out of memory during the build process. <br>

For this reason I've build the images on a [Asus Thinker Board](https://www.asus.com/us/Single-Board-Computer/Tinker-Board/) with 2GB of memory.

## Running the containers
Befor running the containers make sure the directories has been created.

### Running the database;
To run the database container, execute the following code;

````sh
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
````

### Running the webserver;
To run the webserver container, execute the following code;

````sh
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
````

### Running nginx proxy;
To run the nginx container, execute the following code;

````sh
docker run \
  --detach \
  --name nginx \
  --link wordpress \
  --volume /home/[user]/shared/nginx:/etc/nginx/conf.d \
  --volume /home/[user]/shared/wordpress:/var/www/html \
  --publish 80:80 \
  remonlam/docker-wordpress-arm:nginx-armhf
````
