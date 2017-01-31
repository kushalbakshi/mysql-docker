# MySQL for DataJoint
This is a docker image of mysql that comes configured for use with DataJoint!

## How to use

### Using `docker-compose`
The simplest and the recommended way to configure and run a MySQL server with Docker is to use the [Docker compose](https://docs.docker.com/compose/). Once you have `docker` and `docker-compose` installed on your system (the one on which you'd want to run the MySQL server), copy this [`docker-compose.yml`](https://raw.githubusercontent.com/datajoint/mysql-docker/master/docker-compose.yml) to a folder, and run `docker-compose up -d` to start the MySQL server. Here is a series of commands to run in the terminal to achieve this:

```bash
$ mkdir mysql-docker
$ cd mysql-docker
$ wget https://raw.githubusercontent.com/datajoint/mysql-docker/master/docker-compose.yml
$ sudo docker-compose up -d
```

This will start the MySQL server mapped to localhost's port `3306`, and any MySQL data will be stored in the direction `./data`, on if you followed the above instructions, `mysql-docker/data` directory. 

By default the database sets up user `root` with password `simple` (refer to below on how to change this at the startup). You can access your locally running MySQL server as below

```bash
$ mysql -h 127.0.0.1 -u root -p
Enter password: [type in your password here: default is "simple"]

mysql >
```

Read on to find out the details about the content of the `docker-compose.yml` and how you can customize it to fit your needs.

### What's in `docker-compose.yml`?
The content of the `docker-compose.yml` is rather simple:

```yml
version: '2'
services:
  db:
    image: datajoint/mysql
    ports:
      - "3306:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=simple
    volumes:
      - ./data:/var/lib/mysql
```

Let's step through the parts you might want to customize. The line

```yml
- MYSQL_ROOT_PASSWORD=simple
```

configures the default password. If you would like to use something else, you can modify this prior to starting your server with `docker-compose up`.

The lines:

```yml
volumes:
  - ./data:/var/lib/mysql
```

maps the local directory `./data` to the `/var/lib/mysql` inside the container where MySQL stores all of its data by default. If you are running on Mac or Windows, make sure to remove these two lines for you to be able to run the container via `docker-compose up`.