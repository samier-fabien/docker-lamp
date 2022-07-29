# docker-lamp-symfony5

## Table of Contents
1. [What is this ?](#1.what-is-this)
2. [Prerequisite](#2.prerequisite)
3. [Installation](#3.installation)
4. [New symfony project](#4.new-symfony-project)
5. [New project configuration](#5.new-project-configuration)
6. [Then](#6.then)
7. [Git](#7.git)
8. [Clone existing symfony project](#8.clone-existing-symfony-project)


## What is this

Docker-lamp-symfony5 is a full docker configuration for developping a symfony 5 project. You can also use it without the symfony framework as a 'docker-lamp'.

It is composed of:
- **docker_www** container: an apache server with php7.4, composer, nano 
- **docker_db** container: mysql as a database                          
- **docker_phpmyadmin** container: phpmyadmin for managing your database        
- **docker_maildev** container: maildev as a smtp webmail development service

Thanks to Yoan Bernabeu who initially authored the project at [Initial Project](https://gitlab.com/yoandev.co/environnement-de-developpement-symfony-5-avec-docker-et-docker-compose/). Docker-lamp-symfony5 is just a 'repacked' github repository of what he did.



### Prerequisite

Docker and docker-compose installed.



### Installation

Clone docker-lamp-symfony5.
Place your terminal inside the directory you just cloned and launch the docker configuration with:
```
$ docker-compose up -d --build
```


### New symfony project

After installation, create a new project with:
$ docker exec www_docker_symfony composer create-project symfony/skeleton:"^5.4" project
The main directory of your symfony project have to be "project". If you want to change that name, you have to do it inside docker-lamp-symfony5/php/vhosts.conf: wherever there is "project", you have to replace it by the chosen name.
A new project will be created but files are not property of the current user. For becoming the owner, just do:
```
$ sudo chown -R $USER ./
```

Your project is installed now. You can see now the new symfony homepage project at:
**http://127.0.0.1:8741/**

You can access phpmyadmin at:
**http://127.0.0.1:8080/**

Maildev is at:
**http://127.0.0.1:8081/**



### New project configuration

Let's modify the lines of the .env file that look like the following:
```
DATABASE_URL=mysql://root:@db_docker_symfony:3306/db_name?serverVersion=5.7
MAILER_DSN=smtp://maildev_docker_symfony:25
```



### Then

You have to interact with your symfony project. To do this, you have to place your terminal inside the container:
```
$ docker exec -it www_docker_symfony bash
```
Your can interact as usual with symfony and create your database:
```
$ php bin/console doctrine:database:create
```
Now, I won't teach you how to initiate a full symfony project, use the [documentation](https://symfony.com/).
When you want to leave the container:
```
$ exit
```



### Git

Git is installed inside www-docker-symfony container. You can init a project. You can also clone a project from a distant repository.



### Clone existing symfony project

Place your terminal inside your container and clone your project with git. Then as usual :
```
$ composer install
$ php bin/console doctrine:database:create
...
```