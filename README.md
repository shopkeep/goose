goose
=====

This is a docker container for [goose](https://bitbucket.org/liamstask/goose) migrations. This is currently restricted to postgres databases only.  

To use this repository, reference the linked docker image in your Dockerfile:

```
# mounts the local directory as the project db directory for goose
FROM shopkeep/goose
```

Also make sure you pass in the database credentials as environment variables, the container requires the following environment variables:

* DB_HOST - The ip/hostname of the database
* DB_PORT - The port of the database
* DB_NAME - The name of the database
* DB_USER - The username used to connect to the database
* DB_PASSWORD - The users password
* DB_SSL_MODE (optional) - The SSL mode, one of: disable, require (default), verify-full

To build the migrations container, run docker from your apps db directory:
```
# mounts the local directory into the container
$ docker build -t myapp_migration ./

# container exits when complete
$ docker run -i -e "DB_HOST=dbhost" -e "DB_PORT=dbport" -e "DB_NAME=dbname" -e "DB_USER=dbuser DB_SSL_MODE=disable" -e "DB_PASSWORD=dbpass" -t myapp_migration
```

The migrations directory is mounted at build-time, which includes entrypoint, and default CMD args. You can customize what migration is used by overriding the CMD args:

```
$ docker run -i -e "DB_HOST=dbhost" -e "DB_PORT=dbport" -e "DB_NAME=dbname" -e "DB_USER=dbuser" -e "DB_PASSWORD=dbpass" -e "DB_SSL_MODE=require" -t myapp_migration down
$ docker run -i -e "DB_HOST=dbhost" -e "DB_PORT=dbport" -e "DB_NAME=dbname" -e "DB_USER=dbuser" -e "DB_PASSWORD=dbpass" -e "DB_SSL_MODE=require" -t myapp_migration up
```

Instead of passing in environment arguments manually, you can define an environment file and pass it into docker using

```
$ docker run -i --env-file=./envfile -t myapp_migration
```
