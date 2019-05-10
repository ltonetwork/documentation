---
description: Install LTO Network full node plus supporting tools on Apple macOS Mojave.
---

# MacOS

## LTO Network full node

The LTO full node is comprised of a set of [Docker](https://www.docker.com/) containers. For development, use _docker compose_ as orchestration tool.

### Docker Desktop

{% hint style="success" %}
1. Download [Docker Desktop for macOs](https://hub.docker.com/editions/community/docker-ce-desktop-mac).
2. Double-click `Docker.dmg` to open the installer, then drag Moby the whale to the Applications folder.
3. Double-click `Docker.app` in the Applications folder to start Docker.
{% endhint %}

For more detailed instructions please read the [Installation guide in the Docker documentation](https://docs.docker.com/docker-for-mac/install/).

_Docker Desktop includes docker compose, so it's not needed to install that separately._

### LTO full node

```
$ curl "https://raw.githubusercontent.com/legalthings/lto/master/full-node/Docker%20compose/docker-compose.yml" -o docker-compose.yml
$ docker-compose up
```

## Live contracts tester

The live contract tester \(`lctest`\) is build on [Behat](http://behat.org/en/latest/) and runs on [PHP](https://php.net/). It requires PHP 7+ with the  _mongodb_ and _yaml_ PECL extension.

### PHP CLI

PHP 7 is pre-installed on macOS Mojave. Earlier versions of macOS need to update PHP using `brew`.

{% hint style="success" %}
Verify that you have a correct version of PHP by running `php -v` in the terminal.
{% endhint %}

### PECL extensions for PHP

#### MongoDB

_install mongodb client_ \(@sven\)

```text
$ pecl install mongodb
```

#### Yaml

_install libyaml_ \(@sven\)

```text
$ pecl install yaml
```

### lctest.phar

{% hint style="success" %}
1. Download [https://github.com/legalthings/livecontracts-tester/raw/master/lctest.phar](https://github.com/legalthings/livecontracts-tester/raw/master/lctest.phar).
2. Run `php lctest.phar` to verify it's working correctly.
{% endhint %}

