# 📌 devOps-Sciensa
Apresentação de Docker para a sétima semana de treinamento do projeto Trainees 2019 da empresa Sciensa - 24/09/2019
***

# Apresentação
### O que é docker?
  - um conjunto de tecnologias desenvolvido pela dotCloud, futura Docker Inc., que cria um ambiente isolado e facilita o deploy e a execução das nossas aplicações.
  - O Docker é open source

### O que é container?
  - É um ambiente isolado que contém nossa aplicação e funciona junto do nosso SO.

### VM vs Container:
  - VMs possuem SOs, precisam de atualizações frequentes, mantê-las demanda muito tempo.
  - Containers são ágeis na hora de criar e remover aplicações, facilidade para trabalhar com versões diferentes

### Algumas tecnologias do Docker: 
  - Docker Engine, responsável pela ligação entre o SO e os containers.
  - Docker Hub, um repositório cheio de imagens para gente utilizar na nossa infra.

**SUBIR** imagem hello-world 
  ```
  docker run hello-world
  ```
### O que é imagem?
  - É como se fosse uma receita de bolo, são instruções para o docker seguir quando for criar um container.

### Comandos básicos:
  - "docker run" = Executa uma imagem
  - "docker ps" e "docker ps -a" = Lista imagens sendo executadas e lista todas as imagens
  - "docker kill [ID_CONTAINER]" = mata um container, para sua execução
  - "docker start/stop [NOME_IMAGEM]" = inicia e para um container
  - "docker stop $(docker ps -q)" = para todos os containers de uma vez
  - "docker build -f Dockerfile -t [NOME_AUTOR/NOME_IMAGEM] ." = constroi uma imagem (o ponto especifica que o Dockerfile é o do mesmo diretório)
  - "docker rm [ID_CONTAINER]" = remove um container
  - "docker container prune" = apaga todos os containers em estado de "stopped" de uma vez
  - "docker rmi [NOME_IMAGEM]" = remove uma imagem

### Mostrando algumas flags:
**SUBIR** imagem static-site 
   ```
  docker run -d -p 8080:80 dockersamples/static-site
   ```
### Flags de comandos:
  - "-d" = detached, deixa o container rodando em background
  - "-it" = interactive process - exemplo: shell
  - "-p" = indicar qual porta externa e qual porta do container
  - "-v" = criar um volume
***
Agora que vimos os comandos básicos vamos subir um container
  - Criar pasta “teste”, entrar pelo shell.
  
**SUBIR** imagem nginx:1.17.3-alpine
   ```
  docker run -d -p 8080:80 -v $PWD:/usr/share/nginx/html:ro nginx:1.17.3-alpine
   ```
  - criar e altera o arquivvo index.html
  
**SUBIR** imagem ubuntu com flag interativa
 ```
docker run -it ubuntu
 ```
  - executar comando:
```
docker run ubuntu echo "eu estou no container"
```
  - Brincar no terminal
***
### O que é um dockerfile e o que ele contém?
  - Um conjunto de instruções para a criação de uma imagem docker personalizada.

### Conteúdo de um Dockerfile:
```
# ---- Buildando o projeto ReactJS ----
# FROM = Normalmente as imagens são montadas a partir de outras imagens já existentes e no campo e é aqui que
# especificamos a imagem-base que queremos 
# "AS builder" = novo estágio de construção, a imagem criada "builder" poderá ser referenciada neste escopo

FROM node:latest AS builder

# Diretório de trabalho, onde executamos comandos, ponto de entrada da imagem 

WORKDIR /app

# Copiando os arquivos desta (a que está o Dockerfile) pasta para o WORKDIR (/app).

COPY . .

# Rodando comandos para buildar o projeto

RUN yarn install && yarn build

FROM nginx:1.17-alpine

# COPY = copia os arquivos deste diretório para outro diretório
# --from=builder refere-se à imagem criada utilizando o Node no builder do FROM inicial

COPY --from=builder /app/build /usr/share/nginx/html

# LABEL = Metadados
# maintainer = responsável pela imagem

LABEL maintainer="Jorge Hecherat"
```
 - Construindo a imagem e subindo o container

```
docker build -f Dockerfile -t jorge-hecherat/front-git .
docker run -d -p 8080:80 -v $PWD:/usr/share/nginx/html:ro jorge-hecherat/front-git:latest
```

### Exemplo de Dockerfile para aplicação Spring Boot:

```
# Imagem base para rodar aplicação Java (Seria melhor utilizar o jre)
FROM openjdk:8-jdk-alpine

# VOLUME Mecanismo para persistir os dados e compartilhar diretórios gerados no container no SO
# /tmp é onde o Spring cria, por padrão, os arquivos para o Tomcat e o efeito é criar um arquivo temporário no 
# host em "/var/lib/docker" e linkar com o diretório "tmp" do container
VOLUME /tmp

# Expondo a porta 8080 para o mundo fora do container (Tomcat)
EXPOSE 8080

# Variável com o caminho do arquivo .jar
ARG JAR_FILE=target/websocket-demo-0.0.1-SNAPSHOT.jar

# Adicionar "websocket-demo-0.0.1-SNAPSHOT.jar" como "websocket-demo.jar"
ADD ${JAR_FILE} websocket-demo.jar

# Aqui é especificado como a aplicação deve ser executada dentro do container, quais os comandos necessários para a aplicação
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/websocket-demo.jar"]
```

