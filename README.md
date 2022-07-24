## Passos para criação <br>
Usei este artigo como guia:<br>
https://herewecode.io/blog/a-beginners-guide-to-docker-how-to-create-a-client-server-side-with-docker-compose/
<br>
**Criei o ambiente de teste numa vm do Azure:**<br>
*azureuser@docker1:~/testedockercompose$ ls -la<br>
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 .<br>
drwxr-xr-x 6 azureuser azureuser 4096 Jul 24 14:12 ..<br>
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:12 client<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:13 docker-compose.yml<br>
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:12 server*<br>
<br>
**Dentro da pasta server:**<br>
*azureuser@docker1:~/testedockercompose/server$ ls -la<br>
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:15 .<br>
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 ..<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 Dockerfile<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 index.html<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:15 server.py*<br>
<br>
**Arquivo server.py:**<br>
(este código vai levantar um servidor web simples nesta pasta)<br>
https://github.com/szalbuque/dockerCompose/blob/main/server/server.py<br>
<br>
**Arquivo index.html:**<br>
(este arquivo será compartilhado pelo servidor quando ele for iniciado e esta frase será exibida)<br>
https://github.com/szalbuque/dockerCompose/blob/main/server/index.html<br>
<br>
**Arquivo Dockerfile:**<br>
(utiliza a imagem oficial do Python, que está no Docker Hub)<br>
https://github.com/szalbuque/dockerCompose/blob/main/server/Dockerfile<br>
<br>
**Criando o CLIENTE:**<br>
**Na pasta client, criar os arquivos: client.py e Dockerfile.**<br>
*azureuser@docker1:~/testedockercompose/client$ ls -la<br>
drwxrwxr-x 2 azureuser azureuser 4096 Jul 24 14:25 .<br>
drwxrwxr-x 4 azureuser azureuser 4096 Jul 24 14:13 ..<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:25 Dockerfile<br>
-rw-rw-r-- 1 azureuser azureuser    1 Jul 24 14:25 client.py*<br>
<br>
**Arquivo client.py:**<br>
(este código vai pegar o conteúdo da página do servidor web e exibir)<br>
https://github.com/szalbuque/dockerCompose/blob/main/client/client.py<br>
<br>
**Arquivo Dockerfile (na pasta cliente):**<br>
https://github.com/szalbuque/dockerCompose/blob/main/client/Dockerfile<br>
<br>
**Na pasta raiz do projeto, editar o arquivo docker-compose.yml:**<br>
https://github.com/szalbuque/dockerCompose/blob/main/docker-compose.yml<br>
<br>

**Instalar o docker-compose:**<br>
*sudo apt install docker-compose*<br>
<br>
**Usar o comando Docker-Compose para criar a imagem, desta vez com dois containers:**<br>
*docker-compose build*<br>
<br>
*azureuser@docker1:~/testedockercompose$ sudo docker-compose build*<br>
Building server<br>
Step 1/4 : FROM python:latest<br>
latest: Pulling from library/python<br>
Digest: sha256:ce21f64c4c3ae5743ddd5f4d4d9ca5614fddcc4f8c6e32ff2a7ff9a2e8744e8d<br>
Status: Downloaded newer image for python:latest<br>
 ---> 930516bcf910<br>
Step 2/4 : ADD server.py /server/<br>
 ---> 86335ef8a76d<br>
Step 3/4 : ADD index.html /server/<br>
 ---> 15f00a4413a6<br>
Step 4/4 : WORKDIR /server/<br>
 ---> Running in 0fac77e21dd8<br>
Removing intermediate container 0fac77e21dd8<br>
 ---> 0a4fe41a9b14<br>
Successfully built 0a4fe41a9b14<br>
Successfully tagged testedockercompose_server:latest<br>
Building client<br>
Step 1/3 : FROM python:latest<br>
 ---> 930516bcf910<br>
Step 2/3 : ADD client.py /client/<br>
 ---> 64a868be4892<br>
Step 3/3 : WORKDIR /client/<br>
 ---> Running in 8cc3bbc53176<br>
Removing intermediate container 8cc3bbc53176<br>
 ---> f51ca862dad9<br>
Successfully built f51ca862dad9<br>
Successfully tagged testedockercompose_client:latest<br>
<br>
**Usar o comando docker-compose up para iniciar os serviços (containers):**<br>
*azureuser@docker1:~/testedockercompose$ sudo docker-compose up*<br>
Creating network "testedockercompose_default" with the default driver<br>
Creating testedockercompose_server_1 ... done<br>
Creating testedockercompose_client_1 ... done<br>
Attaching to testedockercompose_server_1, testedockercompose_client_1<br>
server_1  | 172.18.0.1 - - [24/Jul/2022 14:42:59] "GET / HTTP/1.1" 200 -<br>
client_1  | $ Docker-Compose is magic!<br>
client_1  |<br>
testedockercompose_client_1 exited with code 0<br>
<br>
**Para testar, abri outro terminal no mesmo servidor e digitei: curl http://localhost**<br>
*azureuser@docker1:~$ sudo curl http://localhost*<br>
$ Docker-Compose is magic!<br>

