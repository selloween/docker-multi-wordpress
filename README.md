# Run multiple WordPress Docker containers with NGINX Proxy, LetsEncrypt and PHP Composer

## Requirements

* Docker
* Docker Compose

## NGINX Proxy and LetsEncrypt

Run `docker-compose up -d` in the `nginx` directory to start the NGINX Proxy and LetsEncrypt Proxy
Companion containers. These containers will handle https.  

## WordPress Setup

* Copy the `wordpress_01` directory for each WordPress site you want to host. I created a copy called `wordpress_02` as an example. If you only want to host one site you can delete `wordpress_02` and modify `wordpress_01` to your liking.

* In each site directory is a `sample.env` - copy that file, edit the environment variables and
rename it to `.env`. Each site directory must contain this environment file.
* Make sure that the container names  are unique for each each site. (`DB_CONTAINER WP_CONTAINER COMPOSER_CONTAINER`). 
* I recommend choosing different database credentials for each site.
* Make optional changes to `wp-config.php`
* The `wp-content` folder is mounted locally to `/srv/www/${VIRTUAL_HOST}/wp_content` for
persistency. This folder contains themes, plugins and uploads.
* Database files are mounted here: `/srv/www/${VIRTUAL_HOST}/db_data

## Custom Theme & Plugin Development
* You can develop a custom theme in `wordpress_XX/theme` or a custom plugin in
* `wordpress_XX/plugin`. Theme and plugin are named after the according environment variable defined in `.env` (`WP_THEME` and `WP_PLUGIN`)


## Composer

Add plugins and/or theme dependencies to `wordpress_XX/composer/composer.json`
They will be installed by the composer container on `docker-compose up --build` I've added common plugins from https://wpackagist.org/ repository as an example.

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

## Build and run containers. 

For each site navigate to its directory and:

``docker-compose up --build``
