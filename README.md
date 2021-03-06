# Drupal 7

# Auto configuration parameters :

- DATABASE_HOST=localhost       ( Name of the mariadb service )
- DATABASE_PORT=3306            ( Port of the mariadb service )
- DRUPAL_DB_NAME=drupal         ( Name of the drupal database )
- DRUPAL_DB_USERNAME=drupal     ( Username to connect to the drupal database )
- DRUPAL_DB_PASSWORD=password   ( Password to connect to the drupal database )
- ADMIN_USERNAME=admin          ( Drupal admin username )
- ADMIN_PASSWORD=password       ( Drupal admin password )
- ADMIN_EMAIL=admin@exemple.org ( Drupal admin email )
- SITE_NAME="drupal"            ( Drupal site name )
- TRUSTED_HOST=localhost        ( Drupal trusted domain )

# Compose file exemple

```

version: '3.1'

services:

  drupal:
    image: jonathancabezas/drupal
    environment:
      - DATABASE_HOST=mariadb
      - DATABASE_PORT=3306
      - DRUPAL_DB_NAME=drupal
      - DRUPAL_DB_USERNAME=drupal
      - DRUPAL_DB_PASSWORD=password
      - SITE_EMAIL=email@example.com
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=password
      - ADMIN_EMAIL=admin@example.com
      - TRUSTED_HOST=172.17.0.1:8080
    ports:
      - 8080:80
    volumes:
      - /tmp/drupal:/var/www/drupal/
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
      - DB_0_NAME=drupal
      - DB_0_PASS=password
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
    mariadb-data:
    mariadb-config:

```