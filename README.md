# docker-lamp-symfony5



## 1.What is this ?

Docker-lamp-symfony5 is a full docker configuration for developping a symfony 5 project. You can also use it without the symfony framework as a 'docker-lamp'.

It is composed of:
-an apache server with php7.4, composer, nano   container name:   **docker_www**
-mysql as a database                            container name:   **docker_db**
-phpmyadmin for managing your database          container name:   **docker_phpmyadmin**
-maildev as a smtp webmail development service  container name:   **docker_maildev**

Yoan Bernabeu initially authored the project at: https://gitlab.com/yoandev.co/environnement-de-developpement-symfony-5-avec-docker-et-docker-compose. Docker-lamp-symfony5 is just a 'repacked' github repository of what he did.



### 2.Prerequisite

Docker and docker-compose installed.



### 3.Installation

Clone docker-lamp-symfony5.
Place your terminal inside the directory you just cloned and launch the docker configuration with:
```
$ docker-compose up -d --build
```


### 4.New symfony project

After installation, create a new project with:
$ docker exec www_docker_symfony composer create-project symfony/skeleton:"^5.4" project
The main directory of your symfony project have to be "project". If you want to change that name, you have to do it inside docker-lamp-symfony5/php/vhosts.conf: wherever there is "project", you replace by the chosen name.
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



### 5.New project configuration

Let's modify the lines of the .env file that look like the following:
```
DATABASE_URL=mysql://root:@db_docker_symfony:3306/db_name?serverVersion=5.7
MAILER_DSN=smtp://maildev_docker_symfony:25
```



### 6.Then ?

You have to interact with your symfony project. To do this, you have to place your terminal inside the container:
```
$ docker exec -it www_docker_symfony bash
```
Your can interact as usual with symfony and can create your database:
```
$ php bin/console doctrine:database:create
```
Now, I won't teach you how to initiate a full symfony project, use the documentation.
When you want to leave the container:
```
$ exit
```



### 7.Git

Git is installed inside www-docker-symfony container. You can init a project. You can also clone a project from a distant repository.



### 8.Clone existing symfony project

Place your terminal inside your container and clone your project with git. Then as usual :
```
$ composer install
$ php bin/console doctrine:database:create
...
```