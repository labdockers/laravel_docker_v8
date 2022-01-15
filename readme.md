****
[FOLLOW NO INSTAGRAM](https://www.instagram.com/wesllycode/)
[LINKEDIN](https://www.linkedin.com/in/weslly-sousa-a0bb2647/)
[DEV.TO](https://dev.to/wesllycode)
[TWITTER](https://twitter.com/wesllycode)

# SOBRE O PROJETO
- Uma estrutura completa para criar um ambiente no docker para se trabalhar com Laravel.


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

Clone o repositório para criar um projeto do Laravel mais recente
```sh
git clone https://github.com/laravel/laravel.git example-project
```

Vamos copiar os arquivos docker-compose.yml, Dockerfile e o diretório docker para o nosso 
projeto recente criado.
```sh
cp -r laravel_docker_v8/* example-project/
```

Acesse a pasta do seu projeto (example-project) e vamos criar o arquivo *.env*
```sh
cp -r .env.example .env
```

Acesse a pasta **example-project** e vamos fazer as configurações das
variáveis de ambiente no seu arquivo *.env*
```dosini
APP_NAME=WesllyCodeLaravel8
APP_URL=http://localhost:8180

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=posso_colocar_nome_que_eu_quiser_do_db
DB_USERNAME=root
DB_PASSWORD=root

CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379
```


----
Uma observação importante,  DB_HOST, CACHE_DRIVER, REDIS_HOST todos eles tem que ter o nome igual do seu container que está configurando no docker-compose. Automaticamente o docker faz referência e identifica o IP do container e acrescenta no ambiente de variável do .env, por isso é suficiente só colocar nome do container em vez do ip.

----

Vamos fazer o deploy dos containers do projeto
```sh
docker-compose up -d
```

Para acessar o container
```sh
docker-compose exec nomedomeuprojeto bash
```

Execute os seguintes comandos
```sh
composer install
```
depois
```sh
php artisan key:generate
```
e depois
```sh
php artisan config:cache
```

- Para acessar a sua aplicação  http://localhost:8180
- Para acessar o phpmyadmin http://localhost:8181

## ALGUMAS ALTERAÇÕES QUE VOCÊ PODE PERSONALIZAR
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


No docker-compose, você pode alterar o nome do network de acordo com o nome do seu projeto, para ficar mais fácil
identificar o projeto que está trabalhando.
```sh
- laravel-nomedomeuprojeto
```

Você pode mudar o nome do container do seu projeto, veja o exemplo abaixo

```sh
 nomedomeuprojeto:
        build: 
            args: 
```
para
```sh
 projeto_laravel:
        build: 
            args: 
```
Não esqueça de atualizar também no arquivo docker/nginx/laravel.conf
```sh
fastcgi_pass nomedomeuprojeto:9000;
```
para
```sh
fastcgi_pass projeto-laravel:9000;
```