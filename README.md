# MariaDB + PHPMyAdmin on Alpine Linux + S6 Overlay

# Auto configuration parameters :

- DATABASE_HOST=localhost ( Name of the mariadb service  )
- DATABASE_PORT=3306 ( Port of the mariadb server )
- DATABASE_PASSWORD=password ( Password of the mariadb server )
- DATABASE_USERNAME=username ( Username of the mariadb server )
- DATABASE_NAME=nextcloud ( Name of the database on the mariadb server )
- ADMIN_USERNAME=admin ( Nextcloud admin username )
- ADMIN_PASSWORD=password ( Nextcloud admin password  )
- TRUSTED_DOMAIN_0=localhost  ( Nextcloud trusted domain )
- TRUSTED_DOMAIN_1=box.exemple.org  ( You can add many trusted domains by adding TRUSTED_DOMAIN_X = domain )



# Compose file exemple

```

version: '3.1'

services:

  nextcloud:
    image: samirkherraz/nextcloud
    environment:
        - DATABASE_HOST=mariadb
        - DATABASE_PASSWORD=password
        - DATABASE_USERNAME=root
        - DATABASE_NAME=nextcloud
        - ADMIN_USERNAME=admin
        - ADMIN_PASSWORD=password
        - TRUSTED_DOMAIN_0=box.exemple.org
    ports:
      - 8080:80
    volumes:
      - nextcloud-data:/var/www/nextcloud/
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global


  mariadb:
    image: samirkherraz/mariadb
    environment:
      - ROOT_PASSWORD=password
    ports:
      - 3306:3306
      - 8081:80
    volumes:
      - mariadb-data:/var/lib/mysql/
      - mariadb-config:/etc/mysql/
    networks:
      default:
    deploy:
      resources:
        limits:
          memory: 256M
      restart_policy:
        condition: on-failure
      mode: global



volumes:
    nextcloud-data:
    mariadb-data:
    mariadb-config:

```