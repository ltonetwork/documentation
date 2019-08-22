# Ubuntu Linux

## LTO Network Anchor Node

The LTO full node is comprised of a set of [Docker](https://www.docker.com/) containers. For development, use _docker compose_ as orchestration tool.

### Docker

```text
$ apt install docker
```

Use `pip` \(python package manager\) to install _docker compose_.

```text
$ pip install docker-compose
```



### LTO full node

```
$ curl "https://raw.githubusercontent.com/ltonetwork/lto-anchor-node/master/docker-compose.yml" -o docker-compose.yml
$ docker-compose up
```

