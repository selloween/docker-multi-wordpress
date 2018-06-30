# A simple Docker Setup for WordPress Development

This setup is focused on WordPress development using Docker on Linux, but should work fine on MacOS or Windows.

## Preface

### Requirements

* Docker
* Docker Compose

### Docker Containers used

* MariaDB https://hub.docker.com/_/mariadb/
* Wordpress https://hub.docker.com/_/wordpress/
* Composer https://hub.docker.com/_/composer/

## Project Structure

```
/configs
    /composer
        composer.json
        Dockerfile
    /wordpress
        Dockerfile
        wp-config.php
/plugin
    index.php
/theme
    index.php
    style.css
.env
.gitignore
docker-compose.yml
```

## Environment variables

Change the enviroment variables in the .env file to suit your project.
See an example below:
```
COMPOSE_PROJECT_NAME=my-awesome-project
DB_NAME=wp-database
DB_USER=wp-user
DB_PASSWORD=db-password
DB_HOST=db
WP_TABLE_PREFIX=wp_
DB_CONTAINER=db-container
WP_CONTAINER=wp-container
COMPOSER_CONTAINER=composer-container
THEME_NAME=my-awesome-theme
PLUGIN_NAME=my-awesome-plguin
```

## Composer

Add plugins and/or theme dependencies to ``config/composer/composer.json``
They will be installed by the composer container on ``docker-compose up --build``
I've added common plugins from https://wpackagist.org/ repository as an example.

```
{
    "name": "wordpress-docker-composer",
    "description": "WordPress Docker & Composer Setup",
    "repositories":[
        {
            "type":"composer",
            "url":"https://wpackagist.org"
        }
    ],
    "require": {
        "wpackagist-plugin/wordpress-seo":"*",
        "wpackagist-plugin/wp-super-cache":"*"
    }
}
```

## Theme/Plugin location

Place your theme/plugin files in the theme/plugin folder. The corresponding theme/plugin folder in the container will be renamed to the name you specified in ``.env`` file.

## Additonal configuration

### Custom wp-config.php

You can add customizations to ``./config/wordpress/wp-config.php``.
Note: You don't have to add database credentials as this is done by the WordPress Docker image automatically!

Note: You have to run ``docker-compose up`` adding the ``--build`` flag to make wp-config changes take effect.

## Persistent Data

volumes are located on Linux hosts under the default location 
``/var/lib/docker/volumes``

``wp_content`` contains all files from ``wp-content``
including uploads etc.

``db_data`` contains database data, so that data won't get lost if the database container gets removed.

## Run Project

``docker-compose up --build``
