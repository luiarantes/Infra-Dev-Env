# Ambiente de Desenvolvimento Docker para PHP e MySql

**Descrição:** O Objetivo é ter ao final um ambiente totalmente funcional para desenvolver em PHP e MySql utilizando imagens de containers oficiais e versionando no GitHub.

**Requisitos:**

Sistema Operacional: **Ubuntu Server 20.04 LTS**

**Pacotes:**

* vim
* git
* docker
* docker compose

### Preparando o ambiente

##### Atualizando sistema operacional, instalando requisitos e reinicializando

```
$ sudo apt update
$ sudo apt upgrade -y
$ sudo apt install vim git apt-transport-https ca-certificates curl software-properties-common mysql-client-core-8.0 -y

$ sudo shutdown -r now
```

##### Instalando Docker

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
$ sudo apt update
$ sudo apt install docker-ce -y
$ sudo usermod -aG docker ${USER}
$ exit
```
Obs: Fazer logout e login novamente para utilização do docker sem root

##### Instalando Docker Compose

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

##### Criando infraestrutura (PHP e MySql) com Docker Compose

```
$ git clone https://github.com/luiarantes/docker-dev-env-php-mysql.git
$ cd ~/docker-dev-env-php-mysql/
$ docker-compose up -d
```

### Versionamento utilizando GIT

##### Definir nome de usuário e endereço de e-mail

```
$ git config --global user.name "Name"
$ git config --global user.name

$ git config --global user.email "email@example.com"
$ git config --global user.email
```

###### Obs: Retirar --global para configurar repositório específico.

##### Clonando aplicação PHP + MySql HelloWorld

```
$ cd ~
$ sudo git clone https://github.com/luiarantes/PHP-MySql-HelloWorld.git
$ sudo chown ${USER}:${USER} PHP-MySql-HelloWorld/ -R
```
Obs: O diretório "PHP-MySql-HelloWorld" foi criado ao subir os containers pelo docker-compose, por isso foi necessário utilizar o sudo ao realizar o clone, nele será possível programar localmente utilizando o editor favorito.

##### Criando arquivos de conexão com o banco de dados

```
$ echo "<?php \$HOST=\"db\"; \$USER=\"root\"; \$PASS=\"mysql.rootp4ss\"; \$DB=\"mysqldb\"; ?>" > ~/PHP-MySql-HelloWorld/db-vars.php
```

##### Conectando ao banco de dados, criando tabela e inserindo registros
```
$ docker exec -it docker-dev-env-php-mysql_db_1 mysql -u root -p
Enter password: mysql.rootp4ss

mysql> USE mysqldb;

mysql> CREATE TABLE tabela (id int(11) NOT NULL AUTO_INCREMENT, coluna varchar(255) NOT NULL, PRIMARY KEY (id));

mysql> INSERT INTO tabela (coluna) VALUES ('Hello World');

mysql> exit
```

##### Acessando a aplicação

```
$ curl 127.0.0.1
```

##### Adicionando arquivos a serem ignorados nos próximos commits.

```
$ echo ".gitignore" > .gitignore
$ echo "db-vars.php" >> .gitignore
```
Obs: O arquivo de conexão do banco de dados será recriado ao realizar o deploy para produção

##### Link para continuidade de criação do ambiente de produção no Elastic Beanstalk
[https://github.com/luiarantes/Infra-Prod-Env](https://github.com/luiarantes/Infra-Prod-Env)
