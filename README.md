goose
=====

This is a docker container for [goose](https://bitbucket.org/liamstask/goose) migrations. To use this repository, reference the linked docker image in your Dockerfile:

```
# mounts the local directory as the project db directory for goose
FROM shopkeep/goose
```

Also make sure you pass in the database credentials as environment variables:

```
$ pwd
myapp/db/

$ ls -l
migrations/
dbconf.yml
Dockerfile

# mounts the local directory into the container
$ docker build -t myapp_migration ./

# container exits when complete
$ docker run -i -e "DB_HOST=dbhost" -e "DB_PORT=dbport" -e "DB_NAME=dbname" -e "DB_USER=dbuser" -e "DB_PASSWORD=dbpass" -t myapp_migration
```

The migrations directory is mounted at build-time, which includes entrypoint, and default CMD args. You can customize what migration is used by overriding the CMD args:

```
$ docker run -i -e "DB_HOST=dbhost" -e "DB_PORT=dbport" -e "DB_NAME=dbname" -e "DB_USER=dbuser" -e "D
B_PASSWORD=dbpass" -t myapp_migration down
B_PASSWORD=dbpass" -t myapp_migration up
```

Instead of passing in environment arguments manually, you can define an environment file and pass it into docker using

```
$ docker run -i --env-file=./envfile -t myapp_migration
```
