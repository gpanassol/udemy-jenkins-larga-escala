# Jenkins e Docker

Acesse nesse [link](https://gitlab.com/rocha.public/cursos/jenkins-em-larga-escala/-/wikis/02-Jenkins-e-Docker)

## Anotações

1º Instalação do Ambiente [Docker](https://docs.docker.com/get-docker/)

2º Dentro da pasta recursos-v0-v1, execute o comando:

```
docker build . --tag <usuario-docker-hub>/missao-devops-jenkins:0.1.0
```

PS.: Necessário cadastro no [Docker Hub](https://hub.docker.com/)

3º Logar no docker hub

```
docker login
```

4º Rodar o container

```
docker run --name docker-jenkins -p 8080:8080 <usuario-docker-hub>/missao-devops-jenkins:0.1.0
```

5º Logar no Jenkins

```
http://localhost:8080/
```

## Próxima versão (Aula 09)

Agora o objetivo é instalar um Jenkins, igual a versão 1 mas com um arquivo de instação (contendo todos os plugins). Arquivo esta dentro da pasta recursos-v0-v1 > v.0.2.0 > plugins.txt

Buildando:

```
docker build . --tag gpanassol/missao-devops-jenkins:0.2.0
```

(As vezes dá erro para baixar algumas libs, tentei varias vezes ate conseguir)

Rodando:

```
docker run -p 8080:8080 gpanassol/missao-devops-jenkins:0.2.0
```

## Aula 10 - E nossas configurações Jenkins

Configurar o Jenkins

1º Clique em admin, muda a senha pra admin/admin

2º Cliente em jenkins (seta para baixo) > Gerenciar Jenkins > Global Tool Configuration > Adicionar JDK 
<br/>Nesse passo adiciona o JDK <b>QUE JÁ ESTA INSTALADO</b> /usr/lib/jvm/java-8-openjdk-amd64


## Aula 11 - Automação de backups e Recovery

Usar o arquivo dentro de /recursos-v0-v1/v.0.3.0

Buildando:

```
docker build . --tag gpanassol/missao-devops-jenkins:0.3.0
```

(As vezes dá erro para baixar algumas libs, tentei varias vezes ate conseguir)

Rodando:

```
docker run --name docker-jenkins-3 \
    -p 8080:8080 \
    -v jenkins_home_3:/var/jenkins_home \
    -v jenkins_backup_3:/srv/backup \
    gpanassol/missao-devops-jenkins:0.3.0
```

1. Login na Ferramenta
2. Troca de Senha
    <br/>clica em admin (usuário em cima lado direito)
    <br/>modifica a senha para admin123

3. Configuração da JDK
    <br/>Jenkins > Manager > Global Tool Configuration
    <br/>Adicionar JDK > desflega 'Instalar automicativamente' (já esta instalado no container Dokcer) 
    <br/>Em nome: 'java-8-openjdk-amd64'
    <br/>Em JAVA_HOME: '/usr/lib/jvm/java-8-openjdk-amd64' (previamente configurado no DockerFile como ENV)
    <br/>
    <br/>Nota.: Talvez de erro de versão no 'Gerenciar o Jenkins' - ignora por hora.

4. Criação de 2 Jobs 
    <br/>Freestyle e Build
    <br/>    Execute shell
    <br/>        sleep 60
    <br/>        sleep 120

5. Configurar Backup
    <br/>Jenkins > Manager > ThinBackup (plugin)
    <br/>Backup directory /srv/backup
    <br/>flegar 'next build number'
    <br/>flegar 'Move old backups'
    <br/>
    <br/>depois clica em Backup Now (é bem rapido)
    <br/>ver no log
    <br/>
    <br/>o Jenkins vai criar a na pasta compartilhada (/var/lib/docker/volumes/jenkins_backup_3/_data)

6. Executar Backup

7. Obter Backup
