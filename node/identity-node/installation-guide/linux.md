# Linux

The LTO identity node is comprised of a set of [Docker](https://www.docker.com/) containers. Use _docker-compose_ as orchestration tool.

### Install docker

{% tabs %}
{% tab title="Ubuntu" %}
```text
apt install docker
```
{% endtab %}

{% tab title="RHEL/CentOS" %}
```
yum install docker
```
{% endtab %}
{% endtabs %}

Use `pip` \(python package manager\) to install _docker-compose_.

```text
$ pip install docker-compose
```

### Start the node

```
$ curl "https://raw.githubusercontent.com/ltonetwork/lto-identity-node/master/docker-compose.yml" -o docker-compose.yml
$ docker-compose up
```

