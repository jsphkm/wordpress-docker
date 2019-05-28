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
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```

> - `db_data` persists Wordpress data on to the mysql database
> - Wordpress Multisite works only on ports `80` and `443`

# Build the project
Run `docker-compose up -d`
> The `-d` flag will run the container in a detached mode, which allows you to run other commands while the container continues to run on the server.

# Install Wordpress
Open `localhost:8000` in the browser and proceed with the Wordpress installation.

# Shutdown and cleanup
- Run `docker-compose down` to remove container and default network, but retain the Wordpress database
- Run `docker-compose down --volumes` to remove container, default network, and the Wordpress database