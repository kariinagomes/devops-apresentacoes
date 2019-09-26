
**Docker Compose**
*****- utilizando o docker-compose do curso da alura*****

Subindo múltiplos containers:
  docker run -d --name meu-mongo --network minha-rede mongo
  docker run --network minha-rede -d -p 8080:3000 douglasq/alura-books:cap05

Mas fazer dessa forma, na mão, não é bom…é mais suscetível a erros, como, por exemplo, acabar executando na ordem errada…

Mas precisa subir tudo na mão?? NÃO! É aí que entra o docker-compose.

É um arquivo .yml, onde estará toda a descrição do que quero que aconteça, todo o processo de subir a aplicação. 
É tipo uma receita de bolo que tem que ser seguida pelo docker quando for subir a aplicação;

A primeira coisa que precisamos incluir no docker-compose.yml é a versão;
```
version: '3'
```
Em seguida colocamos os services: temos nginx, node e mongodb; e em cada um deles precisamos informar como fazer o build; 
 ```
version: '3'
  
services: 
  nginx:
    build: 
 ```
	
O serviço vai ser construído através de um dockerfile, então passamos o caminho de onde ele está; 
e informamos para onde o dockerfile deve ser buscado no contexto;
```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
```
Com a imagem construída, devemos dar um nome para ela; 
e quando o docker-compose criar um container a partir dessa imagem, dará o nome que está no container-name;
```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: karinag/nginx
  ```
E quando o docker compose criar um container a partir dessa imagem, incluir o nome nginx;
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: karinag/nginx
    container_name: nginx
```

Todos nós sabemos que o nginx trabalha com as portas 80 e 443, mas só utilizaremos a porta 80 
(mapeamos da porta 80 para a 80 da minha máquina); 
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: karinag/nginx
    container_name: nginx
    ports:
      - "80:80"
 ```
E para os containers conseguirem se comunicar, eles precisam estar na mesma rede. Como a rede não é um serviço, precisamos nos atentar para a tabulação;
O nome da rede será production-network e brdge;
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: karinag/nginx
    container_name: nginx
    ports:
      - "80:80"
 networks: 
  production-network:
    driver: bridge
 ```

Após criar a rede, podemos incluir essa rede no serviço;
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: karinag/nginx
    container_name: nginx
    ports:
      - "80:80"
    networks: 
      - production-network
networks: 
  production-network:
    driver: bridge
  ```

Como o mongo vai ser a partir da imagem mongo, não precisa incluir a propriedade build.
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: douglasq/nginx
    container_name: nginx
    ports:
      - "80:80"
    networks: 
      - production-network
  mongodb:
    image: mongo
    networks:
      - production-network
 networks: 
  production-network:
    driver: bridge

 ```
Agora incluímos a aplicação node (o cara da caelum quer  três por causa do loadbalancer, então a gente aceita); Aqui indicamos o dockerfile, contexto, rede e porta 3000 (é a porta que está configurada na aplicação);
 ```  
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: douglasq/nginx
    container_name: nginx
    ports:
      - "80:80"
    networks: 
      - production-network
  mongodb:
    image: mongo
    networks:
      - production-network
  node1:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-1
    ports: 
      - "3000"
    networks: 
      - production-network
  node2:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-2
    ports: 
      - "3000"
    networks: 
      - production-network
  node3:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-3
    ports: 
      - "3000"
    networks: 
      - production-network
networks: 
  production-network:
    driver: bridge
 ```
Agora precisamos indicar a ordem da aplicação, utilizando depends_on:
 ```
version: '3'
  
services: 
  nginx:
    build: 
      dockerfile: ./docker/nginx.dockerfile
      context: .
    image: douglasq/nginx
    container_name: nginx
    ports:
      - "80:80"
    networks: 
      - production-network
    depends_on: 
      - "node1"
      - "node2"
      - "node3"
  mongodb:
    image: mongo
    networks:
      - production-network
  node1:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-1
    ports: 
      - "3000"
    networks: 
      - production-network
    depends_on: 
      - "mongodb"
  node2:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-2
    ports: 
      - "3000"
    networks: 
      - production-network
    depends_on: 
      - "mongodb"
  node3:
    build:
      dockerfile: ./docker/alura-books.dockerfile
      context: .
    image: douglasq/alura-books
    container_name: alura-books-3
    ports: 
      - "3000"
    networks: 
      - production-network
    depends_on: 
      - "mongodb"
networks: 
  production-network:
    driver: bridge
 ```
Agora podemos executar para criar as imagens:
```
docker-compose build
```
Executar para começar a receita do bolo:
```
docker-compose up 
```
ou se quiser deixar rodando em background:
```
docker-compose up -d
```
Listar:
```
docker-compose ps
```
Parar e remover:
```
docker-compose down
```
