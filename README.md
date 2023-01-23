# Easy Docker Local Dev Setup

This package provides an easy way to quickly set up php development server with docker. The advantage is you can use multiple project with the same setup. 


## Requirements

The current package requirements are:

- Docker engine
- Docker compose

## Installs

- nginx
- php8.1
- php7.4
- mariadb:latest

You can install your required version easily.

## Instalaltion Steps

- Create src directory.
- In the src directory create your project directory
- In nginx conf.d create virtual domain
- All projects will be at `/var/www` directory in nginx container
- `docker compose up -d`

## Commands inside container
```
docker exec -it -w /var/www/project_id php8.1-container-name pwd
```