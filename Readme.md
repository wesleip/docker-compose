# Criando contêineres com PHP, MariaDB e PHPmyAdmin utilizando um arquivo docker-compose

### Download das imagens em https://hub.docker.com 

## Download imagem PHP

~~~bash
$ docker pull php:7.4-apache
~~~

## Download imagem MariaDB
~~~bash
$ docker pull mariadb:latest
~~~

## Download imagem PHPmyAdmin
~~~bash
$ docker pull phpmyadmin:latest
~~~

## Após o download das imagens criar o arquivo docker-compose.yml
~~~bash
$ vi docker-compose.yml
~~~

## Informações para inserir no arquivo docker-compose.yml
~~~bash
version: '3'
 
services:
  db:
    image: mariadb:latest #nome da imagem utilizada na criação do contêiner
    container_name: my_db #nome que sera dado ao contêiner
    environment: # conjunto de variáveis passadas na configuração do MariaDB
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: create_your_db
      MYSQL_USER: your_user_here
      MYSQL_PASSWORD: create_password
    ports: #porta utilizada para acesso ao banco de dados
      - "3306:3306"
    volumes:
      - ./data:/var/lib/mysql
  
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: my_phpmyadmin
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
    ports:
      - 8081:80
    
  web:
    container_name: my_php
    build: .
    ports:
      - "80:80"
      - "443:443"
    volumes: #volume que será montado na visualização da URL do servidor web
      - /home/Projects/repo:/var/www/html
    links:
      - db
~~~