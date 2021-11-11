---
description: Install LTO Network full node plus supporting tools on Ubuntu Linux 18.04+.
---

# Ubuntu Linux

## LTO Network full node

The LTO full node is comprised of a set of [Docker](https://www.docker.com) containers. For development, use _docker compose_ as orchestration tool.

### Docker

```
$ apt install docker
```

Use `pip` (python package manager) to install _docker compose_.

```
$ pip install docker-compose
```

### LTO full node

```
$ curl "https://raw.githubusercontent.com/legalthings/lto-deepdive/master/docker/dev/docker-compose.yml" -o docker-compose.yml
$ docker-compose up
```

## Live contracts tester

The live contract tester (`lctest`) is build on [Behat](http://behat.org/en/latest/) and runs on [PHP](https://php.net). It requires PHP 7+ with the  _mongodb_ and _yaml _PECL extension.

### PHP

```
$ apt install php-cli php-mongodb php-yaml php-curl php-libsodium
```

### lctest.phar

```
$ curl "https://github.com/legalthings/livecontracts-tester/raw/master/lctest.phar" -o lctest.phar
$ php lctest.phar --version
```
