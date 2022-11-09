# PostgreSQL Docker Plus

This builds on the official [PostgreSQL docker image](https://hub.docker.com/_/postgres/) to add the following plugins:

* [pghashlib](https://github.com/markokr/pghashlib)
* [plproxy](https://plproxy.github.io/)

## Process for creating a Dockerhub image for a new Postgres version

* Create or checkout the branch for the major version of Postgres.
  (example: `git checkout 14`).
* Update the `Dockerfile` with the specific point release of Postgres
  (example: `14.3`).
* Build the image using the process documented [below](#building-manually).
* Update the `Dockerfile` and repeat previous step as necessary until success.
* Commit changes.
* Create a tag for the point release (example: `git tag 14.3`).
* Push the branch and tags (example: `git push; git push --tags`).
* Verify build is queued on [Dockerhub](https://hub.docker.com/repository/registry-1.docker.io/dimagi/docker-postgresql/builds).

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
