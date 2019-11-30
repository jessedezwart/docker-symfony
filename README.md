# Docker Symfony 5.0 (PHP7.3-FPM - NGINX - MySQL - phpMyAdmin)

## Introduction
This is a fork of <https://github.com/maxpou/docker-symfony>, adjusted to my own needs. At the time of writing, the aforementioned repo did not support Symfony 4.3 and 5.0, so I changed some Nginx config. This docker-compose config also uses PHP 7.3 instead of 7.0.
Elk was removed, phpMyAdmin was added.

## Prerequisites
You need to have Docker and docker-compose installed. These following commands are for installing these if you're on Debian. If you're not, just Google and there'll be a guide for your distro.

1. Install Docker
```bash
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
2. Install docker-compose
```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```
3. Clone/download this repo
```bash
$ sudo apt-get update
$ sudo install git
$ git clone https://github.com/jessedezwart/docker-symfony.git
```
## Installation
1. Create a `.env` from the `.env.dist` file. Adapt it according to your symfony application
```bash
$ cp .env.dist .env
```
2. Move your whole Symfony application to the application folder.
3. Change permissions of the application
```bash
$ chmod -R 755 ./application
$ chmod -R 777 ./application/var/
```
4. Run containers
```bash
$ docker-compose up -d
```

## Useful commands
```bash
# bash commands

$ docker-compose exec php bash

  

# Composer (e.g. composer update)

$ docker-compose exec php composer update

  

# SF commands (Tips: there is an alias inside php container)
$ docker-compose exec php php /var/www/symfony/bin/console cache:clear

# Same command by using alias

$ docker-compose exec php bash

$ sf cache:clear

  

# Retrieve an IP Address (here for the nginx container)

$ docker inspect --format '{{ .NetworkSettings.Networks.dockersymfony_default.IPAddress }}'  $(docker ps -f name=nginx -q)

$ docker inspect $(docker ps -f name=nginx -q) | grep IPAddress

  

# MySQL commands

$ docker-compose exec db mysql -uroot -p"root"

  

# Correct permissions

$ sudo chmod -R 777 var/cache var/log

  

# Check CPU consumption

$ docker stats $(docker inspect -f "{{ .Name }}" $(docker ps -q))

  

# Delete all containers

$ docker rm $(docker ps -aq)

  

# Delete all images

$ docker rmi $(docker images -q)
```