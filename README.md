# Docker

🐳 Sejam bem-vindos ao treinamento DOCKER! 🐳 

Vamos conhecer alguns comandos e suas aplicações.

___
## Pré-requisitos

✔ Docker Instalado!

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




Prof. Patrick J. Cardoso 





