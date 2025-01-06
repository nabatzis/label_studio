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
```label-studio start my_project --init -db postgresql```

1. How do we do the init if we are running label-studio within a container? Perhaps connect to the container interactively?
2. How do ew verify, i.e. tables created?


### Environment variables needed
```
DJANGO_DB=default
POSTGRE_NAME=postgres
POSTGRE_USER=postgres
POSTGRE_PASSWORD=
POSTGRE_PORT=5432
POSTGRE_HOST=db
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