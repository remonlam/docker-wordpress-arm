# Wordpress stack for ARM devices

## Still a work in progress


## staring the stuff;

### Running the database;
docker run -d --name database -e MYSQL_ROOT_PASSWORD=rootpassword123 -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=password123 -p 3306 mariadb-arm

### Running the webserver;
docker run -d --name wordpress -e WORDPRESS_DB_HOST=database -e WORDPRESS_DB_NAME=wordpress -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=password123 -p 9000:9000 wordpress-alpine

### Running nginx proxy;
docker run -d -l wordpress --name nginx-proxy -p 80:80 nginx:alpine
