# PHP-Docker-Environment

Lightweight PHP docker development environment

A simple solution for php developers who want to keep their OS clean and safe.
Every service runs in its own container and the database runs inside an isolated network so it has no contact with the outside world.
Webserver and php are running in different containers so the webserver does not calling php's interpreter every time a request arrives instead it provides it to phpfpm container only if needed 

- Apache2
- PHPfpm
- MariaDB
- Composer
- XDebug


### Prerequisites

You need to install docker and docker-compose.Please refer to the official pages below

- [Docker](https://docs.docker.com/install/)
- [Docker-compose](https://docs.docker.com/compose/)


### Installing

First change directory to PHP-Docker-Environment and open .env file for modifications
```
cd PHP-Docker-Environment && cp .env.example .env
nano .env
```
Then you have to replace the existing environment variables values with your own
```
APPLICATION_PATH=/my/absolute/project/path
MYSQL_USER=mydbuser
MYSQL_PASSWORD=mydbuserpassword
MYSQL_DATABASE=mynewdatabase

```

**Remember that your project must contains a folder named public, which will be your document root**

By default apache's and phpfpm's container will run with your user's uid and gid
You can create a new user and a group and override the values with the new ones

```
# create new user and a new group 
sudo useradd testuser && groupadd testgroup

# add your new user to the new group 
sudo usermod -a -G testgroup testuser

# You must also add your current user to the new group for permission reasons
sudo usermod -a -G testgroup $USER

# find new user's id and search group's id
 sudo id -u testuser
 cat /etc/group
 
# Update UID and GID to .env file
GID=myNewGroupId
UID=myNewUserId

```

Finally fire up && build docker-compose

```
docker-compose up --build

```

## Running

Start docker-compose services

```
docker-compose up -d
```

Stop docker-compose services 

```
docker-compose down
```

## Composer

PHP Composer is by default installed into the phpfpm container.
In order to use it you have first find php fpm container id.

```
docker ps -a
```

Then you can execute your composer command as follows below.
By default working directory is your project's path.

```
docker exec 'your php fpm container id here' composer 'your command here'
```

## Author

 **Nikos Rigas** 

