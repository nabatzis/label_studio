# Running Label Studio using docker
```
docker pull heartexlabs/label-studio:latest
```

See below for better way to start,
```
docker run -it -p 8080:8080 \
    -v /Users/nikolaosabatzis/Development/computer_vision/label_studio/mydata:/label-studio/data \
    heartexlabs/label-studio:latest
```

As this is a non-root container, the mounted files and directories must have the proper permissions for the UID 1001.

## Configure use of PostgreSQL
The tables will be created assuming a Postgresql server is running and we have access to it from the docker container and the DB configuration has been updated.

Postgresql files:

**postgresql.conf** - to allow access from the docker container IPs we want or '*'

**pg_hba.conf** - to allow the specific user to the specific DB, i.e.

 | TYPE | DATABASE | USER | ADDRESS | METHOD |
 |------|----------|------|---------|--------|
|host | nikolaosabatzis	| nikolaosabatzis | 192.168.0.156/32 | trust|


### Environment variables needed

For the DB **always** use server address, we are running within a container so localhost, etc. does not work update postgresql.conf file for access from container update pg_hba.conf with the user entry for the user used to connect (see above):



```
DJANGO_DB=default
POSTGRE_NAME=postgres
POSTGRE_USER=postgres
POSTGRE_PASSWORD=
POSTGRE_PORT=5432
POSTGRE_HOST=192.168.0.156
```

Alternatively we can start the docker container with:

```
docker run -it -p 8080:8080 \
    -v /Users/nikolaosabatzis/Development/computer_vision/label_studio/mydata:/label-studio/data \
    -e POSTGRES_HOST=<your_postgres_host> \
    -e POSTGRES_PORT=<your_postgres_port> \
    -e POSTGRES_USER=<your_postgres_user> \
    -e POSTGRES_PASSWORD=<your_postgres_password> \
    -e POSTGRES_NAME=<your_postgres_db_name> \
    heartexlabs/label-studio:latest
```
In our case we have a `docker-compose-ls.yml`.

# Install Label Studio without internet access
Download label-studio docker image (host with internet access and docker):

```
bash
docker pull heartexlabs/label-studio:latest
```
Export it as a tar archive:
```
bash
docker save heartexlabs/label-studio:latest | gzip > label_studio_latest.tar.gz
```
Transfer it to another VM:
```
bash
scp label_studio_latest.tar.gz <ANOTHER_HOST>:/tmp
```
SSH into <ANOTHER_HOST> and import the archive:
```
bash
docker image import /tmp/label_studio_latest.tar.gz
```