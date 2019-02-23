In order to install a WordPress development environment on [Docker](https://www.docker.com/), you'll need to install Docker and [docker-compose](https://docs.docker.com/compose/).

Once you have those installed, install your WordPress in the folder of your choice, and add `docker-compose.yml` file to the root of your WordPress installation folder

```yml
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
      WORDPRESS_DB_NAME: db_name # pull this from env variables
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

After adding that file and changing the database name, run

`docker-compose up -d`

This will build a Docker container and image with the specified settings from the `.yml` file. Alternatively, you can run

`docker-compose up -d && docker-compose logs -f wordpress`

which is useful because it gives you WP log output.

For more information about setting up and using Docker with WordPress, click [here](https://docs.docker.com/samples/wordpress/).

Another way of using Docker, which is especially useful when working on continuous integration and deployment (CI/CD), is to use `Dockerfile`. For instance, one such Dockerfile can look like this

```dockerfile
# Stage 0, build app
FROM php:7.2-fpm as build-container

RUN curl -sS https://getcomposer.org/installer | php \
  && chmod +x composer.phar && mv composer.phar /usr/local/bin/composer

RUN apt-get update && apt-get install -y gnupg
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install -yq nodejs build-essential \
        git unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
    && curl -sL https://deb.nodesource.com/setup_8.x | bash - \
    && pecl install mcrypt-1.0.1 \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) mysqli

RUN npm install -g npm

WORKDIR /build
COPY . /build
RUN cp /build/wp-config.php.template /build/wp-config.php
RUN bash /build/scripts/build-plugins.sh

# Stage 1, build app container
FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        unzip \
        mysql-client \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install gd mysqli opcache zip

ADD https://downloads.wordpress.org/release/wordpress-4.9.8-no-content.zip /var/www/latest.zip
RUN cd /var/www && unzip latest.zip && rm latest.zip
RUN rm -rf /var/www/html
RUN mkdir -p /var/www/html/ \
    && mv /var/www/wordpress/* /var/www/html/

# Copy wp files
COPY --from=build-container /build/ /var/www/html/
RUN chown www-data:www-data /var/www/html/ -R
COPY config/php.ini /usr/local/etc/php/
WORKDIR /var/www/html/

CMD ["exec","php-fpm"]
```

This Dockerfile will first build a local container with php, composer, and node, and in that stage your theme/plugins can be built. Notice the `RUN bash bin/build-plugins.sh` command. You can replace this with your installation script (be it for plugins or theme). You'll place composer and npm builds in that shell script.

After that, you'll build the WordPress container, and copy the plugins built in the previous stage. You can modify this to your liking. Beside this, you'll probably need a Docker image for nginx. But we won't be going into too much detail about it.

If you named this file `Dockerfile`, you'll run

```bash
docker build -t yourtag .
```

In case you named it something like `Dockerfile.php` (if you need multiple stages and builds), you'll run

```bash
docker build -t yourtag -f Dockerfile.php .
```

The Dockerfiles should be located in the root of your project.

To go in the interactive shell of the built image, type

```bash
docker run -it CONTAINER_NAME bash
```

If that fails, you can try

```bash
docker run -d CONTAINER_NAME
docker exec -it CONTAINER_NAME bash
```

Running the `docker run` with `-d` means you are detaching it, and you can check the logs created while building it.

## Useful Docker commands

Here are some useful Docker commands that you might use.

List all containers

`docker ps -as`

List all images

`docker images`

Stop all running containers

`docker stop $(docker ps -aq)`

Remove all containers

`docker rm $(docker ps -a -q)`

Remove all images

`docker rmi $(docker images -q)`

Prune the system from unused images, containers, and networks [documentation link](https://docs.docker.com/config/pruning/#prune-everything)

`docker system prune`

### Docker notes

Depending on your project, the Docker compose or Dockerfile may differ. For more information, consult your friendly devops.
