# Docker and Wordpress
These are modified directions from
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-docker-compose

## Set up a repository for your project in git following the
[a Getting Started with Git and GitHub](../../getting-started/getting-started-git.md)

## For the Docker setup for the development environment
cd into the repository directory

``` 
mkdir docker-compose
cd docker-compose
mkdir nginx
cd nginx
touch nginx.conf
```

Then make the following changes to nginx.conf.  Replace your_domain with the domain name that this
will be published on.  You can modify your /etc/hosts file (MacOS) to have this domain resolve to your
localhost instead of the what is browseable on the web.  You can always uncomment that line when you want to
see the public facing site.

This approach was taken because Wordpress doesn't really play all the nicely with changing internal links
and site domain name well after initial installation.  If you start out with the domain name other than what it will be in the end,
you will need to run sql update commands on the Wordpress database when you switch the domain.  Not saying
you can't do this, but just wanted to simplify things.

```
server {
listen 80;
listen [::]:80;

        server_name example.xyz;

        index index.php index.html index.htm;

        root /var/www/html;
        include mime.types;
        location ~ /.well-known/acme-challenge {
                allow all;
                root /var/www/html;
        }

        location / {
                try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass wordpress-tim:9000;
                fastcgi_index index.php;
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_param PATH_INFO $fastcgi_path_info;
        }

        location ~ /\.ht {
                deny all;
        }

        location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
                expires max;
                log_not_found off;
        }
}
```

You will want to add the nginx.conf to your repo.


Next, create a `.env` file in the top-level directory of your repo

Add
```
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_USER=your_wordpress_database_user
MYSQL_PASSWORD=your_wordpress_database_password
```

You will not want to add this to your repo as it contains sensitive data.

Also, add this file to the [a .gitignore](wordpress-sample-gitignore.md) and [a .dockerignore](wordpress-sample-dockerignore.md) file


Next, create a docker-compose.yml

```
version: '3'

services:
  db-tim:
    image: mysql:8.0
    container_name: db-tim
    restart: unless-stopped
    env_file: .env
    environment:
      - MYSQL_DATABASE=wordpress
    ports:
      - "23233:3306"
    volumes:
      - dbdata-tim:/var/lib/mysql
    command: '--default-authentication-plugin=mysql_native_password'
    networks:
      - app-network-tim

  wordpress-tim:
    depends_on:
      - db-tim
    image: wordpress:5.1.1-fpm-alpine
    container_name: wordpress-tim
    restart: unless-stopped
    env_file: .env
    environment:
      - WORDPRESS_DB_HOST=db-tim:3306
      - WORDPRESS_DB_USER=$MYSQL_USER
      - WORDPRESS_DB_PASSWORD=$MYSQL_PASSWORD
      - WORDPRESS_DB_NAME=wordpress
    volumes:
      - wordpress-tim:/var/www/html
    networks:
      - app-network-tim

  webserver:
    depends_on:
      - wordpress-tim
    image: nginx:1.15.12-alpine
    container_name: webserver
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - wordpress-tim:/var/www/html
      - ./docker-compose/nginx:/etc/nginx/conf.d
      - ./src/www:/var/www/html
    networks:
      - app-network-tim

volumes:
    wordpress-tim:
    dbdata-tim:

networks:
    app-network-tim:
      driver: bridge
```

For listing the docker containers running
`docker ps`

For debugging a docker container service

`docker-compose logs service_name` 

For recreating the container
`docker-compose up --force-recreate <container_name>`


You may need to change the permissions for the default user on the database, in which case use

```
CREATE USER 'your-user-here'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your-password-here';
GRANT ALL PRIVILEGES ON wordpress.* TO 'your-user-here'@'localhost';
```

For Git setup,
.gitignore

For the Wordpress setup

TODO: SSL self-signed on the Docker Container so that https show up


May need to run these commands in the webserver terminal    
chown www-data:www-data  -R *
find . -type d -exec chmod 755 {} \;  
find . -type f -exec chmod 644 {} \;





