# phalcon-docker-stack

A set of docker-compose files suitable for starting a scalable Phalcon project.

One of the goals is to ensure that the docker-compose file(s) used in production are used as much as possible in development, in contrast to using separate files with a lot of similar information.

## Examples of Using the docker-compose files

### Full Dev Stack

The majority of the docs assume all services are running from the *Full Dev Stack*. This makes getting started as a develop easier.
If you are supporting a production its expected you'll know when to replace your connection with the Dev Stack info contained here.

```
docker-compose \
    -f docker-compose.yml \
    -f docker-compose.override.yml \ 
    -f docker-compose.web.yml \
    -f docker-compose.postgres.yml \
    -f docker-compose.devtools.yml \
    up
```

### Run only the PHP-FPM Phalcon container

```docker-compose up```

Note: This will use the *default* `docker-compose.yml` and `docker-compose.override.yml`


### Run an Nginx proxy, PHP-FPM Phalcon and Devtools containers

This is useful if you are developing against an existing database.

```
docker-compose \
    -f docker-compose.yml \
    -f docker-compose.override.yml \ 
    -f docker-compose.web.yml \
    -f docker-compose.devtools.yml \
    up
```

### Run only Nginx proxy and PHP-FPM Phalcon 

This is useful if you already have logging and DB admin tools running.

```
docker-compose \
    -f docker-compose.yml \
    -f docker-compose.override.yml \ 
    -f docker-compose.web.yml \
    up
```

## docker-compose files

### docker-compose.yml

Defines the core image used for to provide a Phalcon PHP7.1-FPM container.

### docker-compose.override.yml

Settings for the core Phalcon PHP7.1-FPM container.
*You may want to tweak this*

### docker-compose.web.yml

Defines an Nginx proxy container with a link to the PHP container.

### docker-compose.postgres.yml

Defines a PostgreSQL container and corresponding links in PHP and Adminer

### docker-compose.devtools.yml

Defines Adminer and Logspout containers

## Services

### PHP - Phalcon PHP7.1-FPM 

Runs on port 9000 internally.
It does not expose any ports.
it's connected to via the `php` link.

### WEB - Nginx Proxy to FastCGI

Host port of 8880
This provides proxied access to the PHP-FPM container.
The [Phalcon Vokuro](https://github.com/phalcon/vokuro) demo app is installed to get developers up and running.


### POSTGRES - PostgreSQL database 

Runs on port 9000 internally.
It does not expose any ports unless you remove the comments from the docker-compose file..
it's connected to via the `postgres` link.

### ADMINER - Database Manager

A great database access tool to let you see your local and remote database.

### WEBTOOLS - Phalcon Webtools

The Phalcon Webtools

### LOGGER - Logspout exposing logs via HTTP

LogSpout is the easiest way to aggregate Docker STDIN and STDOUT output for viewing or sending to some place like Papertrail.
The default config provide here exposes the local docker logs via HTTP.