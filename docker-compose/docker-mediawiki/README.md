# About this Docker Compose file

This compose file aim to create Mediawiki web services, and not
build all those service in one-container-mode.
Seperate container mode could replace any dependence service, such as:
- Replace MySQL to PostgreSQL
- Upgrade/Downgrade MySQL version
- Use PHPMyAdmin to manage MySQL DB

Even use Nginx or Apache to make the frontend proxy server.

# Usage

Make sure the [Docker Compose](https://docs.docker.com/compose/)
was installed in host environment already. Or reference to the
[official installation document](https://docs.docker.com/compose/install/).

## Startup Web Services
Execute below command to startup Mediawiki web services with docker
compose:
```console
cd /path/to/docker-compose-file
docker-compose up
```

## Copy LocalSettings.php

Copy LocalSettings.php into container via `docker cp`, please follow:
```
CONTAINER_ID=`docker ps -a|grep 'synctree\/mediawiki'|cut -d' ' -f1`
docker cp /path/to/docker-compose/LocalSettings.php $CONTAINER_ID:/var/www/html
```
*Note*: `docker cp` could copy between Host and container after Docker
1.8, before Docker 1.8, `docker cp` only support to copy file from
container to Host.

For more `docker cp` command information, please follow this
[link](https://docs.docker.com/engine/reference/commandline/cp/)

Alternative solution, use `docker inspect` and `cp`:
```console
SHORT_CONTAINER_ID=`docker ps -a|grep 'synctree\/mediawiki'|cut -d' ' -f1`
FULL_CONTAINER_ID=`docker inspect -f '{{.Id}}' $SHORT_CONTAINER_ID`
sudo cp /path/to/LocalSettings.php /var/lib/docker/aufs/mnt/**$FULL_CONTAINER_ID**/var/www/html
```

## Access Web Service

Docker containers already expose port to host environment, so just
access these links to control the new systems:
- Mediawiki: http://localhost:8080
- PHPMyAdmin: http://localhost:8081
