
# docker-php7-redis-composer

dockerfile with PHP 7.3-fpm-alpine, phpredis 4.2.0 and lates composer.
included PHP modules:

Dockerfile: https://github.com/devoloperegrem/docker-php7-redis-composer

 - intl
 - xml
 - pdo_mysql
 - opcache

## supported tags

 - :lates for amd64 architecture
 - :armhf for arm

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


