
[FOLLOW NO INSTAGRAM](https://www.instagram.com/wesllycode/)
[LINKEDIN](https://www.linkedin.com/in/weslly-sousa-a0bb2647/)
[DEV.TO](https://dev.to/wesllycode)
[TWITTER](https://twitter.com/wesllycode)

# SOBRE O PROJETO
- Uma estrutura completa para criar um ambiente no docker para se trabalhar com Laravel.
- Uma observação, que você tem duas maneiras de fazer esse tutorial, optei em criar uma pasta myapp para
  o código fica mais organizado, mas você pode colocar toda aplicação do laravel junto com arquivos do
  docker-compose e dockerfile e pode até deletar o arquivo .env.example e modificar as variáveis direto
  .env


# ESTRUTURA
 * [Laravel](https://laravel.com)
 * [nginx:alpine](https://hub.docker.com/_/nginx) - [versions](https://nginx.org/en/CHANGES)
 * [php:8.0-fpm](https://hub.docker.com/_/php)
 * [redis:alpine](https://hub.docker.com/_/redis)
 * [phpmyadmin:latest](https://hub.docker.com/_/phpmyadmin)
 * [Mysql 5.7.22](https://hub.docker.com/_/mysql)
 * [Node 14 LTS](https://github.com/nodesource/distributions#debmanual)


 # PROCEDIMENTO A SER EXECUTADO

Clone o repositório 
```sh
git clone https://github.com/labdockers/laravel_docker_v8.git
```

Clone os Arquivos do Laravel
```sh
git clone https://github.com/laravel/laravel.git example-project
```
Agora vamos criar uma pasta myapp na pasta laravel_docker_v8
```sh
mkdir laravel_docker_v8/myapp

```

Depois copie os arquivos da pasta example-project para pasta myapp que está dentro da pasta laravel_docker_v8
```sh
cp -r example-project/* laravel_docker_v8/myapp
```

A configuração das variáveis do  seu arquivo .env.example deve ser parecidas, caso mude o nome do container no seu docker-compose tem que mudar aqui também.
```sh
APP_NAME=NomeDaMinhaAplicacao
APP_URL=http://localhost:8180

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel_aqui_eu_posso_mudar_o_nome
DB_USERNAME=root
DB_PASSWORD=root

CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
```

Vamos acessar a pasta laravel_docker_v8 e vamos copiar o arquivo .env.example para .env  para dentro da pasta do myapp
```sh
cp -r .env.example myapp/.env
```


----
Uma observação importante,  DB_HOST, CACHE_DRIVER, REDIS_HOST todos eles tem que ter o nome igual do seu container que está configurando no docker-compose. Automaticamente o docker faz referência e identifica o IP do container e acrescenta no ambiente de variável do .env, por isso é suficiente só colocar nome do container em vez do ip.

----

No docker-compose, você pode alterar o nome do network de acordo com o nome do seu projeto, para ficar mais fácil
identificar o projeto que está trabalhando.          
```sh
- laravel-nomedoprojeto
```

Outra mudança que você pode fazer no docker-compose, e alterar o nome para de acordo com o nome do seu projeto, veja o exemplo abaixo:

```sh
 laravel_8:
        build: 
            args: 
```
para
```sh
 nome_do_meu_projeto:
        build: 
            args: 
```


Vamos fazer o deploy dos containers do projeto
```sh
docker-compose up -d
```

Para acessar o container
```sh
docker-compose exec nome_do_meu_projeto bash
```
Instalar as depedências do projeto, no inicio não precisa, pois o dockerfile já faz isso quando está montando os container pela primeira vez.
```sh
composer install
```

Gerar a key para o projeto no Laravel, no inicio também não precisa, pois o dockerfile já faz isso quando está montando os container pela primeira vez. 
```sh
php artisan key:generate
```

Para acessar a sua aplicação  http://localhost:8180
Para acessar o phpmyadmin http://localhost:8181

# É POSSÍVEL ALTERAR AS PORTAS ?
Sim, é só ir nas configurações docker-compose e alterar as portas do serviço.
Por exemplo, quero de 8181

```sh
ports:
        - 8181:80
```
para 8182
```sh
ports:
        - 8182:80
```