# docker-lamp-symfony5

## Table of Contents
1. [What is this ?](#1-what-is-this)
2. [Prerequisite](#2-prerequisites)
3. [Installation](#3-installation)
4. [New symfony project](#4-new-symfony-project)
5. [New project configuration](#5-new-project-configuration)
6. [Then](#6-then)
7. [Git](#7-git)
8. [Clone existing symfony project](#8-clone-existing-symfony-project)
9. [Connect database](#9-connect-database)
10. [Lack of something](#10-lack-of-something)


## 1. What is this

Docker-lamp-symfony5 is a full docker configuration for developping a symfony 5 project. You can also use it without the symfony framework as a 'docker-lamp'.

It is composed of:
- **docker_www** container: an apache server with php7.4, composer, nano 
- **docker_db** container: mysql as a database                          
- **docker_phpmyadmin** container: phpmyadmin for managing your database        
- **docker_maildev** container: maildev as a smtp webmail development service

Thanks to Yoan Bernabeu who initially authored the project at [Initial Project](https://gitlab.com/yoandev.co/environnement-de-developpement-symfony-5-avec-docker-et-docker-compose/). Docker-lamp-symfony5 is just a 'repacked' github repository of what he did.



### 2. Prerequisite

Docker and docker-compose installed.



### 3. Installation

Clone docker-lamp-symfony5.
Place your terminal inside the directory you just cloned and launch the docker configuration with:
```
$ docker-compose up -d --build
```


### 4. New symfony project

After installation, create a new project with:
```
$ docker exec docker_www composer create-project symfony/skeleton:"^5.4" project
```
With this command you tell docker you want to "exec" the command "composer create-project symfony/skeleton:"^5.4" project" inside the "docker_www" container.  
The main directory of your symfony project has to be "project". If you want to change that name, you have to do it inside `docker-lamp-symfony5/php/vhosts.conf`: wherever there is `project`, you have to replace it by the chosen name.  
If you want to interact with a container you can do as previously for a single command or you can do as followed:
```
$ docker exec -it docker_www bash
```
Once your terminal "points to" the docker_www container, you are identified as root.  
A new project is created but files are not property of the current user. For becoming the owner, just do:
```
# sudo chown -R $USER ./
```
Your project is installed now. You can see now the new symfony homepage project at:
**http://127.0.0.1:8741/**

You can access phpmyadmin at:
**http://127.0.0.1:8080/**

Maildev is at:
**http://127.0.0.1:8081/**



### 5. New project configuration

Let's modify two lines of the .env file. Open the file in an editor:  
```
# cd project
# nano .env
```
It should look like the following after modification:
```
DATABASE_URL=mysql://root:@docker_db:3306/db_name?serverVersion=5.7
MAILER_DSN=smtp://docker_maildev:25
```
You can replace `db_name` by the name you want for your database.



### 6. Then

Your can interact as usual with symfony and create your database:
```
# composer install
# php bin/console doctrine:database:create
```
Now, I won't teach you how to initiate a full symfony project, use the [documentation](https://symfony.com/).
When you want to leave the container:
```
# exit
```


### 7. Git

Git is installed inside `docker_www` container. You can init a project. You can also clone a project from a distant repository.



### 8. Clone existing symfony project

Place your terminal inside your `docker_db` container and clone your project with git. Then as usual change your `DATABASE_URL` and `MAILERDSN` in the `.env`, then:
```
# composer install
# php bin/console doctrine:database:create
...
```



### 9. Connect database

If you can't connect to database with some software like dbeaver, use:
```
$ docker ps
```
copy the `CONTAINER ID` of the mysql container and:
```
$ docker inspect CONTAINER_ID
```
It will return `"IPAddress": "172.xx.xx.x"` you can also use that command:
```
$ docker inspect CONTAINER_ID | grep -i ipaddress
```
The full address to access your database is `172.xx.xx.x:3306`, just replace `172.xx.xx.x` with the value you previously found.


### 10. Lack of something

If ever you need some php lib or to install vim for example, just modify the php/Dockerfile. Keep in mind your docker_www container is a little debian linux distribution.