# Wordpress stack for ARM devices

## Still a work in progress


## staring the stuff;

### Running the database;

docker run \
  --detach \
  --name mariadb \
  --volume /home/remonlam/shared/mariadb:/var/lib/mysql \
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
  --volume /home/remonlam/shared/wordpress:/var/www/html \
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
  --volume /home/remonlam/shared/nginx:/etc/nginx/conf.d \
  --volume /home/remonlam/shared/wordpress:/var/www/html \
  --publish 80:80 \
  remonlam/docker-wordpress-arm:nginx-armhf
