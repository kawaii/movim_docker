# Quick reference

-	**Where to get help**:
	the Movim XMPP MUC - movim@conference.movim.eu

-	**Where to file issues**:
	[https://github.com/movim/movim_docker/issues](https://github.com/movim/movim_docker/issues)

# What is Movim?

Movim is a distributed social network built on top of XMPP, a popular open standards communication protocol. Movim is a free and open source software licensed under the AGPL. It can be accessed using existing XMPP clients and Jabber accounts. Learn more at [movim.eu](https://movim.eu/).

> [wikipedia.org/wiki/Movim](https://en.wikipedia.org/wiki/Movim)

![logo](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Movim-logo.svg/354px-Movim-logo.svg.png)

# How to use this image

```console
$ docker container run movim/movim:latest
```

This image only provides a Movim service container running PHP7.X-FPM. There are no database, cache or nginx container(s) provided, you'll need to use Docker Compose or Stack to wrange those additional services to your Movim instance.

## ... via [`docker stack deploy`](https://docs.docker.com/engine/reference/commandline/stack_deploy/) or [`docker-compose`](https://github.com/docker/compose)

Example `stack.yml` for `movim`:

```yaml
services:
  movim:
    environment:
      MOVIM_ADMIN: admin
      MOVIM_PASSWORD: password
      MOVIM_DOMAIN: http://localhost
      MOVIM_PORT: 8080
      MOVIM_INTERFACE: 0.0.0.0
      POSTGRES_DB: movim
      POSTGRES_HOST: postgresql
      POSTGRES_PORT: 5432
      POSTGRES_USER: movim
      POSTGRES_PASSWORD: changeme
    image: movim/movim:0.18.1rc0
    volumes:
    - ${PWD}/movim:/var/www/html:rw
  nginx:
    image: nginx:mainline-alpine
    ports:
    - published: 80
      target: 80
    volumes:
    - ${PWD}/movim:/var/www/html:ro
    - ${PWD}/nginx:/etc/nginx/conf.d/:ro
  postgresql:
    environment:
      POSTGRES_DB: movim
      POSTGRES_PASSWORD: changeme
      POSTGRES_USER: movim
    image: postgres:12.4-alpine
    volumes:
    - ${PWD}/postgres/data:/var/lib/postgresql/data:rw
version: '3.7'
```
