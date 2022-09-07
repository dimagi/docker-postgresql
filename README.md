# PostgreSQL Docker Plus

This builds on the official [PostgreSQL docker image](https://hub.docker.com/_/postgres/) to add the following plugins:

* [pghashlib](https://github.com/markokr/pghashlib)
* [plproxy](https://plproxy.github.io/)

## Building manually

```sh
export tag=<version-tag>
export base_image=dimagi/docker-postgresql
export image=$base_image:$tag

# list existing image
docker images "$base_image"

# optional: forcibly remove old container and image
docker rm -fv "$image"
docker rmi "$image"

# build new image
docker build -t "$image" .
```
