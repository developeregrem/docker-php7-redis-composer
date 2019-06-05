
# docker-php7-redis-composer

This Docker image is based on [PHP 7.3-fpm-alpine](https://hub.docker.com/_/php/), [phpredis 4.2.0](https://github.com/phpredis/phpredis) and lates [composer](https://hub.docker.com/_/composer).
This image can be used, for example, when you use compose to manage your php dependencies and you want to use redis as an in-memory data structure as a database cache or to store php sessions.

## Included PHP modules

 - gd
 - intl
 - xml
 - pdo_mysql
 - opcache
 
Dockerfile: https://github.com/developeregrem/docker-php7-redis-composer

## supported tags

 - `:lates` - 64 bit Intel/AMD (x86_64/amd64)
 - `:armhf` - 32 bit ARM (ARMv7/armhf)

## docker-compose example

    version: "3"
    services:
    	web:
            image: nginx:mainline
            ports:
                - 80:80
                - 443:443
            volumes:
                - /path/to/www/dir:/var/www:cached
                - custom/ngnix/conf/dir:/etc/nginx/conf.d/
            networks:
                - internal-network
            restart: always
    
        php:
            image: developeregrem/php7-redis-composer:armhf
            volumes:
                - /path/to/www/dir:/var/www:cached
                - ./conf/php/conf.ini:/usr/local/etc/php/conf.d/conf.ini
            networks:
                - internal-network
            environment:
                - TZ=Europe/Berlin
            restart: always
    	
	    redis:
            image: redis:latest
            restart: always
            volumes:
                 - redis-vol:/data/
            networks:
                - internal-network
    
    		
    networks:
        internal-network:
            driver: bridge
    volumes:
        redis-vol:
	
Also see my full web stack docker-compose example including ACME (letsencrypt) here: [acme-php-nginx-dockerized](https://github.com/developeregrem/acme-php-nginx-dockerized)
		
## Volume structure

 - `/var/www` - web accessible resources (php files)
 - `/usr/local/etc/php` - custom php config file (see example [conf.ini](https://github.com/developeregrem/docker-php7-redis-composer/blob/master/conf/php/conf.ini))

## Environment variables

 - `TZ` - time zone e.g. "Europe/Berlin"
 
 ## Run it
 
 You can easily run the docker-compose example from above which includes all relevant images for a web stack (without database), containing the lates [ngnix](https://hub.docker.com/_/nginx) as web server, [redis](https://hub.docker.com/_/redis) as in-memory cache and this php image.
 To run it 
 - adjust the `volumes` paths from the example to your needs
 - you can use the example php config file from here [conf.ini](https://github.com/developeregrem/docker-php7-redis-composer/blob/master/conf/php/conf.ini)
 
 - `docker-compose up -d`
 
 To install or update composer dependencies run e.g.:
 
  - `docker-compose exec -u www-data php /bin/sh -c "composer update"`
 

