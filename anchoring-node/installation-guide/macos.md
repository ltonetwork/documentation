# MacOS

The LTO anchor node is comprised of a set of two [Docker](https://www.docker.com/) containers. For development, use _docker compose_ as orchestration tool.

### Docker Desktop

{% hint style="success" %}
1. Download [Docker Desktop for macOs](https://hub.docker.com/editions/community/docker-ce-desktop-mac).
2. Double-click `Docker.dmg` to open the installer, then drag Moby the whale to the Applications folder.
3. Double-click `Docker.app` in the Applications folder to start Docker.
{% endhint %}

For more detailed instructions please read the [Installation guide in the Docker documentation](https://docs.docker.com/docker-for-mac/install/).

_Docker Desktop includes docker-compose, so it's not needed to install that separately._

### Start the node

{% hint style="success" %}
1. Download the [docker composer configuration file](https://raw.githubusercontent.com/ltonetwork/lto-anchor-node/master/docker-compose.yml) for LTO anchor node.
2. In Terminal run `docker-composer up`.
{% endhint %}

