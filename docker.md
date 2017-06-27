# Docker setup

To install WordPress development environment on [Docker](https://www.docker.com/) you'll need to install Docker and [docker-compose](https://docs.docker.com/compose/).

Once you have those installed, install your WordPress in the folder of your choice and add `docker-compose.yml` file

```
version: '3'
services:
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - 8080:80
      - 443:443
    volumes:
      - ./data:/data # Required if importing an existing database
      - ./:/var/www/html   # Theme development
    environment:
      WORDPRESS_DB_NAME: db_name # Change to match the one from your wp-config.php
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root
  db:
    image: mysql:5.7
    volumes:
      - data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db_name
      MYSQL_USER: root
      MYSQL_PASSWORD: root
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      MYSQL_ROOT_PASSWORD: root
    ports:
      - 3306:80
volumes:
  data: {}
```

After adding that file and changing the database name run

`docker-compose up -d`

This will build docker container and image with the specified settings from the `.yml` file. Alternatively you can run

`docker-compose up -d && docker-compose logs -f wordpress`

which is useful because it gives you WP log output.

For more information about setting and using docker with WordPress click [here](https://docs.docker.com/samples/wordpress/).

## Useful Docker commands

Here are some useful docker commands that you might use.

List all containers

`docker ps -as`

Stop all running containers

`docker stop $(docker ps -aq)`

Remove all containers

`docker rm $(docker ps -aq)`

Remove all images

`docker rmi $(docker images -q)`
