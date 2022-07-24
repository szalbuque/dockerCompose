Usei este artigo como guia:
https://herewecode.io/blog/a-beginners-guide-to-docker-how-to-create-a-client-server-side-with-docker-compose/

**Criei o ambiente de teste numa vm do Azure:**
*azureuser@docker1:~/testedockercompose$ ls -la
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 .
drwxr-xr-x 6 azureuser azureuser 4096 Jul 24 14:12 ..
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:12 client
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:13 docker-compose.yml
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:12 server*

**Dentro da pasta server:**
*azureuser@docker1:~/testedockercompose/server$ ls -la
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:15 .
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 ..
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 Dockerfile
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 index.html
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 server.py*

**Arquivo server.py:**
(este código vai levantar um servidor web simples nesta pasta)
https://github.com/szalbuque/dockerCompose/blob/main/server/server.py

**Arquivo index.html:**
(este arquivo será compartilhado pelo servidor quando ele for iniciado e esta frase será exibida)
https://github.com/szalbuque/dockerCompose/blob/main/server/index.html

**Arquivo Dockerfile:**
(utiliza a imagem oficial do Python, que está no Docker Hub)
https://github.com/szalbuque/dockerCompose/blob/main/server/Dockerfile

**Criando o CLIENTE:**
**Na pasta client, criar os arquivos: client.py e Dockerfile.**
*azureuser@docker1:~/testedockercompose/client$ ls -la
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:25 .
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 ..
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:25 Dockerfile
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:25 client.py*

**Arquivo client.py:**
(este código vai pegar o conteúdo da página do servidor web e exibir)
https://github.com/szalbuque/dockerCompose/blob/main/client/client.py

**Arquivo Dockerfile (na pasta cliente):**
https://github.com/szalbuque/dockerCompose/blob/main/client/Dockerfile

**Na pasta raiz do projeto, editar o arquivo docker-compose.yml:**
https://github.com/szalbuque/dockerCompose/blob/main/docker-compose.yml


**Instalar o docker-compose:**
*sudo apt install docker-compose*

**Usar o comando Docker-Compose para criar a imagem, desta vez com dois containers:**
*docker-compose build*

*azureuser@docker1:~/testedockercompose$ sudo docker-compose build*
Building server
Step 1/4 : FROM python:latest
latest: Pulling from library/python
Digest: sha256:ce21f64c4c3ae5743ddd5f4d4d9ca5614fddcc4f8c6e32ff2a7ff9a2e8744e8d
Status: Downloaded newer image for python:latest
 ---> 930516bcf910
Step 2/4 : ADD server.py /server/
 ---> 86335ef8a76d
Step 3/4 : ADD index.html /server/
 ---> 15f00a4413a6
Step 4/4 : WORKDIR /server/
 ---> Running in 0fac77e21dd8
Removing intermediate container 0fac77e21dd8
 ---> 0a4fe41a9b14
Successfully built 0a4fe41a9b14
Successfully tagged testedockercompose_server:latest
Building client
Step 1/3 : FROM python:latest
 ---> 930516bcf910
Step 2/3 : ADD client.py /client/
 ---> 64a868be4892
Step 3/3 : WORKDIR /client/
 ---> Running in 8cc3bbc53176
Removing intermediate container 8cc3bbc53176
 ---> f51ca862dad9
Successfully built f51ca862dad9
Successfully tagged testedockercompose_client:latest

**Usar o comando docker-compose up para iniciar os serviços (containers):**
*azureuser@docker1:~/testedockercompose$ sudo docker-compose up*
Creating network "testedockercompose_default" with the default driver
Creating testedockercompose_server_1 ... done
Creating testedockercompose_client_1 ... done
Attaching to testedockercompose_server_1, testedockercompose_client_1
server_1  | 172.18.0.1 - - [24/Jul/2022 14:42:59] "GET / HTTP/1.1" 200 -
client_1  | $ Docker-Compose is magic!
client_1  |
testedockercompose_client_1 exited with code 0

**Para testar, abri outro terminal no mesmo servidor e digitei: curl http://localhost**
*azureuser@docker1:~$ sudo curl http://localhost*
$ Docker-Compose is magic!

