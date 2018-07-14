# Wordpress stack for ARM devices

## Still a work in progress


## staring the stuff;

### Running the database;
docker run -d --name mariadb --volume /home/remonlam/shared/mariadb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=rootpassword123 -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=password123 -p 3306 remonlam/docker-wordpress-arm:mariadb-armhf

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
docker run -d --name wordpress --link mariadb --volume /home/remonlam/shared/wordpress:/var/www/html -e WORDPRESS_TABLE_PREFIX=wp_ -e WORDPRESS_DB_HOST=mariadb -e WORDPRESS_DB_NAME=wordpress -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=password123 -p 9000:9000 remonlam/docker-wordpress-arm:wordpress-armhf





### Running nginx proxy;
docker run -d --link wordpress --volume /home/remonlam/shared/nginx:/etc/nginx/conf.d --volume /home/remonlam/shared/wordpress:/var/www/html --name nginx -p 80:80 remonlam/docker-wordpress-arm:nginx-armhf
