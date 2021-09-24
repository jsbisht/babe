### installing and running the docker redis image (as daemon)

> docker run --name redis-db -p 6666:6379 -d redis:6.2

or as foreground:

> docker run --name redis-db -p 6666:6379 redis:6.2

where,

- redis-db is the name given to the docker image
- 6379 is the port at which the redis is running within the container
- 6666 is the port which is exposed to the outside world

### listing of existing containers

> docker ps

or listing with their names:

> docker ps -a

### stopping a running container

> docker stop YOUR_CONTAINER_ID

or

> docker stop redis-db

### removing a container by name or id

> docker rm YOUR_CONTAINER_ID

or by name:

> docker rm redis-db

### listing docker images

> docker image ls

### removing docker image

> docker image rm -f redis:6.2

## docker cli

> docker exec -it redis-db redis-cli

### Working with redis storage

> set name "Bisht"

> get name "Bisht"

### Working with redis storage with expiry

Setting 10 second expiry:

> set apiurl "http://api.googleapis.com/xyz" EX 10

> get apiurl "http://api.googleapis.com/xyz"

### checking if exists

> exists apiurl

### deleting a key

> del apiurl

### checking list of keys

```
keys *
```

### clearing all the keys

> flushdb

### checking the remaining expiration time of a key

> TTL <key-name>
