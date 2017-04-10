## Usage
**Using docker-compose**
Modify the docker-compose.yml file to point to the correct paths of your media on your server.
```
docker-compose up -d
```

```
docker create \
	--name=plex \
	--net=host \
	-e VERSION=latest \
	-e PUID=<UID> -e PGID=<GID> \
	-v </path/to/library>:/config \
	-v <path/to/tvseries>:/data/tvshows \
	-v </path/to/movies>:/data/movies \
	linuxserver/plex
```

**Parameters**

* `--net=host` - Shares host networking with container, **required**.
* `-v /config` - Plex library location. *This can grow very large, 50gb+ is likely for a large collection.*
* `-v /data/xyz` - Media goes here. Add as many as needed e.g. `/data/movies`, `/data/tv`, etc.
* `-e VERSION=latest` - Permits specific version selection e.g. `0.9.12.4.1192-9a47d21`, also supports `public` (this forces plex so stick with ). If left blank, auto update is disabled until set.
* `-e PGID=` for for GroupID - see below for explanation
* `-e PUID=` for for UserID - see below for explanation

*Special note* - If you'd like to run Plex without requiring `--net=host` then you will need the following ports in your `docker create` command:

```
  -p 32400:32400 \
  -p 32400:32400/udp \
  -p 32469:32469 \
  -p 32469:32469/udp \
  -p 5353:5353/udp \
  -p 1900:1900/udp
```

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" <sup>TM</sup>.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Updates / Monitoring

* Shell access whilst the container is running: `docker exec -it plex /bin/bash`
* Upgrade to the latest version: `docker restart plex`
* Upgrade to the latest version: `docker-compose restart`
* To monitor the logs of the container in realtime: `docker logs -f plex`

## Changelog

