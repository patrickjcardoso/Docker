# Docker

🐳 Sejam bem-vindos ao treinamento DOCKER! 🐳 

Vamos conhecer alguns comandos e suas aplicações.

___
## Pré-requisitos

✔ Docker Instalado!

___
# Aula 01
___




## Primeiros Comandos

* Listar os comandos disponíveis
```
docker
```
ou
```
docker container
```
* Verificar a versão do docker

```
docker version
```
* Listar os container em execução

```
docker container ls
```

* Listar as imagens presentes no host

```
docker image ls
```

* Listar todos os container, inclusive os parados

```
docker container ls -a
```
___
## Trabalhando com Containers

* Executando seu primeiro container
```
docker container run hello-world
```

### Verificar se:
1. O container está em execução?
2. Alguma imagem foi baixada?
3. Listar os containers que forma criados e suas informações.

* Executar um container Linux
```
docker container run ubuntu
```
1. O que aconteceu com esse container? Por que ele é executado e destruido?

### Conhecendo as opções do run 
```
docker container run --help
```

### Executando container em modo interativo

```
docker container run -it debian bash
touch /curso-docker.txt
exit
```

* Execute o comando docker run anterior e verifique se o arquivo criado existe?

### Definindo o nome para seus containers

O nome dos contêineres devem ser únicos, como percebido, o docker cria automáticamento o nome para você. Você pode definir o nome que achar melhor, desde que siga a regra de nomes únicos.

```
docker container run --name meudebian -it debian bash
exit
docker container run --name meudebian -it debian bash
```
* Ao executar o segundo docker run, o que aconteceu?


### Reutilizando Containers

```
docker container ls
docker container ls -a
docker container start -ai meudebian
touch docker.txt
exit
docker container start -ai meudebian
ls docker.txt
exit
```

### Mapeamento de Portas

É possível mapear portas, permitindo o acesso através de toda a rede. O parâmetro -p recebe as duas portas como por exemplo: -p 8080:80 onde o primeiro é do host e o segundo é do container.

```
docker container run -p 8080:80 nginx
```

* Verificar se a porta 8080 está respondendo.

#### Exemplo MySQL

https://hub.docker.com/_/mysql

```
docker pull mysql
```

```
docker container run mysql
```

* Ao executar o comando run, funcionou?

```
docker container run -e MYSQL_ROOT_PASSWORD=root mysql
```

Criando container MySQL com paramêtros e expondo a porta
```
docker container run -e MYSQL_ROOT_PASSWORD=root --name=mysql_server -d -p 3306:3306 mysql
```

* Você pode acessar o Server do MySQL através de um client.


### Mapeando Volumes

*** Atenção: Se você estiver utilizando o Katacoda, pode não funcionar.

[Documentação sobre Volumes](https://docs.docker.com/storage/volumes/)

É possível mapear tanto diretórios no host como entidades especiais conhecidas como volumes para diretórios no container. Por enquanto vamos nos concentrar no mapeamento mais simples, uma diretório no host para um diretório no container. O método mais comum para este fim é o parâmetro -v no comando docker container run, o -v recebe um parâmetro que normalmente é composto por dois caminhos absolutos separados por : (dois pontos). Assim como diversos outros parâmetros, o primeiro é no host e o segundo é no container. (GOMES, 2020)

1. Subir o container e testar o acesso

```
docker container run -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html -d nginx
```

2. Criar o diretório html e criar o arquivo (caso não exista) html/index.html conforme exemplo:
```
<h1> Hello World </h1>
```

3. Acessar o host e verificar as mensagem de saida!

4. Editar o arquivo html com o container rodando e verifica se as alterações foram aplicadas em tempo real.


### Executando containers em Background (modo daemon)

O parâmetro -d do docker container run indica ao Docker para iniciar o container em background (modo daemon).

* Subindo um container em background
```
docker container run -d --name ex-deamon -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html nginx
```

* Listar os containers

* Outras ações com container em Background

```
# reinicar um container
docker container restart <nome ou id>

# Parar um container
docker container stop <nome ou id>

# Iniciar um container
docker container stop <nome ou id>
```


### Outros Comandos
```
docker container ls
docker container ls -a
```

#### Inspecionar container https://docs.docker.com/engine/reference/commandline/inspect/
```
docker container inspect
```

#### Executar um comando em um container em execução https://docs.docker.com/engine/reference/commandline/exec/
```
docker container exec
```

#### Exibir logs de um container https://docs.docker.com/engine/reference/commandline/logs/
```
docker container logs
```

[Documentação sobre Comandos](https://docs.docker.com/engine/reference/commandline/docker/)

___
### Referências: 
GOMES, Rafael. Docker para desenvolvedores. Leanpub: Instruct 9Bravos, 2020. Disponível em: https://leanpub.com/dockerparadesenvolvedores. Acesso em: 01 out. 2021.
ls
___
# Aula 02
___

###Dockerfile 

[Extra Sobre Dockerfile](https://caiodelgado.dev/entendendo-dockerfile/)

## DockerFile Exemplo 01 - Criando sua primeira imagem!
1. Cria um diretório e dentre dele crie seu arquivo Dockerfile com o conteúdo abaixo:

```
FROM alpine:3.4

RUN apk update
RUN apk add vim
RUN apk add curl
```

2. Buildando a imagem

```
docker build -t patrickjcardoso/alpine-leve:1.0 .
```

3. Criando um container da imagem

```
docker run --rm -ti patrickjcardoso/alpine-leve:1.0 /bin/sh
```

### Buffer de imagem

* Susbstitua a linha que contém o curl por git
```
FROM alpine:3.4

RUN apk update
RUN apk add vim
RUN apk add curl
```

* Buld novamenta a imagem e observe as saídas!

### Simplificando o Dockerfiel

```
FROM alpine:3.4

RUN apk update && \
    apk add curl && \
    apk add vim && \
    apk add git
```

OU

```
FROM alpine:3.4

RUN apk update && apk add \
    curl \
    git \
    vim
```
## Dockerfile Exemplo 02 - Exemplo Site Docker

https://docs.docker.com/get-started/02_our_app/





## Docker Compose

### Exemplo 01 - Wordpress e Mysql com Docker Compose

### Ambiente

* Crie uma nova pasta e:
```
mkdir exemplo01 && cd exemplo01
```
* Crie uma pasta chamada wordpress
* Crie outra pasta chamada db
* Crie um arquivo chamado docker-compose.yml
** Cole neste arquivo o conteúdo da próxima seção
```
version: '3.1'
 
services:
 
  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - ./wordpress:/var/www/html
 
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - ./db:/var/lib/mysql
```

[Leitura Recomendada sobre docker-compose](https://blog.4linux.com.br/docker-compose-explicado/)

* Execute docker-compose up
* Acesse localhost:8080

* Abra um novo terminal e verifique os conteineres em execução.
* Verifique também se existem volumes no docker.
* Verifique também se existem redes no docker.

Para encerrar, volte para o terminal e aperte CTRL+C






Prof. Patrick J. Cardoso 

___
### Referências: 
GOMES, Rafael. Docker para desenvolvedores. Leanpub: Instruct 9Bravos, 2020. Disponível em: https://leanpub.com/dockerparadesenvolvedores. Acesso em: 01 out. 2021.



