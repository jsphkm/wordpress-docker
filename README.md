# Wordpress-docker

Create a new project and create `docker-compose.yml` in the root
```yml
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:5.2.1-php7.3-apache
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
   working_dir: /var/www/html
   volumes:
       - ./wp-content:/var/www/html/wp-content
       - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
volumes:
    db_data: {}
```
> - Define custom directories for volumes to create and manage custom themes
> - `db_data` persists Wordpress data on to the mysql database
> - Wordpress Multisite works only on ports `80` and `443`

## Build the project
Run `docker-compose up -d`
> The `-d` flag will run the container in a detached mode, which allows you to run other commands while the container continues to run on the server.

## Install Wordpress
Open `localhost:8000` in the browser and proceed with the Wordpress installation.

## Shutdown and cleanup
- Run `docker-compose down` to remove container and default network, but retain the Wordpress database
- Run `docker-compose down --volumes` to remove container, default network, and the Wordpress database

## Custom themes
Create a new folder inside `wp-content/themes/` and create `index.php`
```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
  <meta charset="<?php bloginfo( 'charset' ); ?>">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <?php wp_head(); ?>
</head>

<body <?php body_class(); ?>>

<div class="wrap">
  <div id="primary" class="content-area">
    <main id="main" class="site-main" role="main">

      <?php
      if (have_posts()) :
        /* Start the Loop */
        while (have_posts()) : the_post();
      ?>
        <div>
          <a href="<?php the_permalink(); ?>"><h3><?php the_title(); ?></h3></a>
        </div>
      <?php
        endwhile;
        /* End the Loop */
      else :
        // Nothing
      endif;
      ?>

    </main><!-- #main -->
  </div><!-- #primary -->
</div><!-- .wrap -->

<?php get_footer(); ?>
<?php wp_footer(); ?>

</body>
</html>
```

Activate the theme in the dashboard