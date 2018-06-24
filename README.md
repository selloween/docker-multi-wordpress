# A simple Docker Compose Setup for WordPress Development including Composer

This setup is focused on WordPress development using Docker on Linux, but should work fine on MacOS or Windows.

## Preface

### File Permissions on Linux 

Docker containers run as the root user on Linux hosts. For that reason volume and bind mounts are created as root and are not writeable by the local user. This can be very frustrating when developing localy and needing to have access to the WordPress theme and plugin directories.

This setup let's you focus on developing your WordPress Theme and/or Plugin without the headaches of changing file permissions, ownerships and UIDs/GIDs by avoding volume bind mounts of default directories - e.g. ``wp-content``and therefore getting permission conflicts out of the way. (Information on using bind mounts: https://docs.docker.com/storage/bind-mounts/)

### Requirements

The only requirement is running Docker and Docker Compose on your system.
This configuration was sucessfully tested using ``Docker version 18.05.0-ce`` and ``docker-compose version 1.21.2``

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

Change the enviroment variabels in the .env file to suit your project.
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
Note: You don't have to add database credentials as this is done by Docker automatically!

Note: You have to run ``docker-compose up`` adding the ``--build`` flag to make configuration changes take effect.

## Persistent Data

volumes are located under the default location 
``/var/lib/docker/volumes``

``wp_content`` contains all files from ``wp-content``
including uploads etc.

``db_data`` contains database data, so that data won't get lost if the containers get removed.

## Run Project

``docker-compose up --build``

Note: You only need to add the ``--build`` flag when running ``docker-compose up`` the first time or if you added changes to configuration files e.g. ``composer.json``
