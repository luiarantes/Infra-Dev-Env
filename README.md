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
$ sudo apt install vim git -y

$ sudo shutdown -r now
```

##### Instalando Docker

```
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
$ sudo apt update
$ sudo apt install docker-ce -y
$ sudo usermod -aG docker ${USER}
```
Obs: Fazer logout e login novamente

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
Obs: O diretório "PHP-MySql-HelloWorld" foi criado ao subir os containers pelo docker-compose, por isso foi necessário utilizar o sudo ao realizar o clone.

##### Criando arquivos de conexão com o banco de dados

```
$ echo "<?php mysqli_connect("db", \"root\", \"mysql.rootp4ss\") or die(mysqli_error()); ?>" >> ~/PHP-MySql-HelloWorld/db.php
```

##### Acessando a aplicação

```
$ curl 127.0.0.1
```

##### Adicionando arquivos a serem ignorados nos próximos commits.

```
$ echo ".gitignore" > .gitignore
$ echo "db.php" >> .gitignore
```
Obs: O arquivo de conexão do banco de dados será recriado ao realizar o deploy para produção
