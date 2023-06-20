# Sobre

Imagem para rodar em produção laravel + vuejs

# Conteúdo da Imagem Docker

- <b>PHP</b>, e diversas extensões e Libs do PHP, incluindo php-redis, pgsql, mysql, entre outras.

- <b>Nginx</b>, como proxy reverso/servidor.

- <b>Supervisor</b>, indispensal para executarmos a aplicação PHP e permitir por exemplo a execução de filas e jobs.

- <b>Composer</b>, afinal de contas é preciso baixar as dependências mais atuais toda vez que fomos crontruir uma imagem Docker.

- <b>Node</b>, para realizar a instlação de pacotes javascript e buildar o vuejs

- <b>Redis</b>, para guardar as filas do laravel

# Passo a Passo

## Certifique-se de estar com o Docker em execução.

```sh
docker ps
```

## Certifique-se de ter o Docker Compose instalado.

```sh
docker compose version
```

## Clone sua aplicação Laravel para a pasta 'app'. Caso a pasta app não existe, crie a pasta.

A listagem de pastas do projeto deve ficar:

```
    app/
    docker/
    .gitignore
    docker-compose.yml
    readme.md
```

## Certifique-se que sua aplicação Laravel ficou em `./app` e que existe o seguinte caminho: `/app/public/index.php`

## Certifique-se que sua aplicação Laravel possuí um .env

- Para buildar a aplicação e subir ela, rode o comando a seguir, esse comando seta o .env que está na pasta app:

```sh
docker compose --env-file app/.env up -d --force-recreate --build
```

## Para derrubar a aplicação e limpar o cache, volume ou qualquer outro dado desenecessário, execute:

```sh
docker system prune -f ; docker volume prune -f ;docker rm -f -v $(docker ps -q -a)
```

## Para entrar dentro do Container da Aplicação, execute:

```sh
docker exec -it web bash
```
