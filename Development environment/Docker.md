In order to install a WordPress development environment on [Docker](https://www.docker.com/), you'll need to install Docker and use [docker-compose](https://docs.docker.com/compose/).

Once you have those installed, in your desired folder add `docker-compose.yml` file


```yml
version: '3.3'

services:

  app:
    image: wordpress:5.2-fpm
    container_name: wp-docker-app
    depends_on:
      - db
    restart: always
    volumes:
      - ./bin/php.ini:/usr/local/etc/php/conf.d/local.ini
      - .:/var/www/html
    environment:
      WORDPRESS_DB_HOST: wp-docker-db
      WORDPRESS_DB_NAME: wordpress-handbook-app
      WORDPRESS_DB_USER: wp
      WORDPRESS_DB_PASSWORD: wp
    expose:
      - "80"

  db:
    image: mysql:5.7
    container_name: wp-docker-db
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_RANDOM_ROOT_PASSWORD: 1
      MYSQL_DATABASE: wordpress-handbook-app
      MYSQL_USER: wp
      MYSQL_PASSWORD: wp
    ports:
      - "33066:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: wp-docker-phpmyadmin
    external_links:
      - db
    ports:
      - 8181:80
    environment:
      MYSQL_RANDOM_ROOT_PASSWORD: 1
      MYSQL_USERNAME: root

  nginx:
    image: nginx
    container_name: wp-docker-nginx
    restart: always
    volumes:
      - ./:/var/www/html
    depends_on:
      - app
    volumes:
      - ./bin/nginx.conf:/etc/nginx/conf.d/default.conf
      - .:/var/www/html
      - ./logs:/var/log/nginx
    ports:
      - 8010:80

volumes:
  db_data:

```

Besides adding `docker-compose.yml` file, create `bin` folder with two files in it: `nginx.conf` and `php.ini` for your nginx and php settings. The `nginx.conf` looks like this

```bash
## secure headers
# https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xxxsp
add_header X-Xss-Protection "1; mode=block" always;
# https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xfo
add_header X-Frame-Options "SAMEORIGIN" always;
# https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#xcto
add_header X-Content-Type-Options "nosniff" always;

## global fastcgi config
fastcgi_hide_header X-Powered-By;
fastcgi_pass_header Authorization;

server {
    listen 80;

    add_header Strict-Transport-Security "max-age=31536000" always;

    root /var/www/html;
    index index.php;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    client_max_body_size 64M;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

And `php.ini` looks like this

```bash
file_uploads = On
memory_limit = 512M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 600
```

After adding those files, from your terminal, run the following command

`docker-compose up -d`

This will build docker container and image with the specified settings from the `.yml` file in the detached state.

For more information about setting up and using Docker with WordPress, click [here](https://docs.docker.com/samples/wordpress/).

You can see if the containers are running by typing 

```bash
docker ps --all
```

You should see something like

```bash
CONTAINER ID    IMAGE                     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
353c53e7721b    nginx                     "nginx -g 'daemon of…"   8 seconds ago   Up 5 seconds   0.0.0.0:8010->80/tcp                    wp-docker-nginx
8cf9f52c0540    wordpress:5.1.1-fpm       "docker-entrypoint.s…"   9 seconds ago   Up 7 seconds   80/tcp, 9000/tcp                        wp-docker-app
e1f1d0799b40    mysql:5.7                 "docker-entrypoint.s…"   10 seconds ago  Up 8 seconds   33060/tcp, 0.0.0.0:33066->3306/tcp      wp-docker-db
352d5f6f64a9    phpmyadmin/phpmyadmin     "/run.sh supervisord…"   10 seconds ago  Up 8 seconds   9000/tcp, 0.0.0.0:8181->80/tcp          wp-docker-phpmyadmin
```

Your project will be available on `http://localhost:8010`. Setting up HTTPS is a bit tricky but doable (you can try it yourself 😄).

Another way of using Docker is to use `Dockerfile`, which is especially useful when working on continuous integration and deployment (CI/CD). For instance, one such Dockerfile can look like this

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

This Dockerfile will first build a local container with php, composer, and node, and in that stage your theme/plugins can be built. It also contains the `RUN bash bin/build-plugins.sh` command. You can replace this with your installation script (be it for plugins or theme). You'll place composer and npm builds in that shell script.

After that, you'll build the WordPress container and copy the plugins built in the previous stage. You can modify this to your liking. Beside this, you'll probably need a Docker image for nginx. But we won't be going into too much detail about it.

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
