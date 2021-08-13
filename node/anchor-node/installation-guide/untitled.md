# Windows

The LTO anchor node is comprised of a set of two [Docker](https://www.docker.com/) containers. Use _docker-compose_ as orchestration tool.

### Docker Desktop

{% hint style="success" %}
1. Download [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows).
2. Double-click `Docker Desktop for Windows Installer.exe` to run the installer.
3. Docker does not start automatically. To start it, search for `Docker` and select _Docker Desktop for Windows_ in the search results.
{% endhint %}

For more detailed instructions please read the [Installation guide in the Docker documentation](https://docs.docker.com/docker-for-windows/install/).

_Docker Desktop includes docker-compose, so it's not needed to install that separately._

### Start the node

{% hint style="success" %}
1. Download the [docker composer configuration file](https://raw.githubusercontent.com/ltonetwork/lto-anchor-node/master/docker-compose.yml) for LTO anchor node.
2. In PowerShell \(or another terminal\) run `docker-composer up`.
{% endhint %}

