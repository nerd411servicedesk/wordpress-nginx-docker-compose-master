
# Docker Compose and WordPress

[![Build Status](https://travis-ci.org/urre/wordpress-nginx-docker-compose.svg?branch=master)](https://travis-ci.org/urre/wordpress-nginx-docker-compose)

Use WordPress with Docker using [Docker compose](https://docs.docker.com/compose/)

+ `Dockerfile` for extending a base image and install wp-cli
	+ Using a custom [Docker image](https://github.com/urre/wordpress-nginx-docker-compose-image) with [automated build on Docker Hub](https://cloud.docker.com/repository/docker/urre/wordpress-nginx-docker-compose-image)
+ Local domain ex `myapp.local`
+ Custom nginx config in `./nginx`
+ Custom `php.ini` config in `./config`
+ Volumes for `nginx`, `wordpress` and `mariadb`
+ WordPress using [Bedrock](https://roots.io/bedrock/) - modern development tools, easier configuration, and an improved folder structure
+ CLI scripts for creating a self signed SSL certificate for using https
+ CLI script for trusting certs in macOS System Keychain
+ CLI script for setting up an entry in your host file

## Setup

### Requirements

Install [Docker](https://www.docker.com/get-started)

### Create SSL cert

```shell
cd cli && ./create-cert.sh
```

> Edit the script to your your custom domain, this example uses myapp.local

### Trust cert in macOS Keychain. (Chrome and Safari will trust the certs, for Firefox: add them in preferences)

```shell
cd cli && ./trust-cert.sh
```

> Edit the script to your your custom domain, this example uses myapp.local

### Setup vhost in /etc/hosts

```shell
cd cli && ./setup-hosts-file.sh
```
> Follow the instructions. For example use `myapp.local`

### Setup ENV

```shell
cd src
cp .env.example .env
```

Use the following database settings:

```yml
DB_HOST=mysql:3306
DB_NAME=myapp
DB_USER=root
DB_PASSWORD=password
```

## Install WordPress and Composer dependencies

```shell
cd src
composer install
```

> You can also use composer like this: `docker-compose run composer update`

## Run

```shell
docker-compose up -d
```

🚀 Open up [https://myapp.local](https://myapp.local)


### Notes:

> When making changes to the Dockerfile : Use `docker-compose up -d --force-recreate --build`.

### Tools

#### wp-cli

```
docker exec -it myapp-wordpress bash
wp search-replace https://olddomain.com https://newdomain.com --allow-root
```

#### Useful Docker Commands

Login to the docker container

```shell
docker exec -it myapp-wordpress bash
```

Stop

```shell
docker-compose stop
```

Down (stop and remove)

```shell
docker-compose down
```

Cleanup

```shell
docker-compose rm -v
```

Recreate

```shell
docker-compose up -d --force-recreate
```

Rebuild docker container when Dockerfile has changed

```shell
docker-compose up -d --force-recreate --build
```
