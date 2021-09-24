# Docker

## Docker Commands

To view version of docker client and server:

> docker version

To view info about versions of different components within docker:

> docker info

To run a new container:

> docker run &lt;image-name&gt;

To see running and stopped containers:

> docker ps

To see info about images:

> docker images

### `docker run` command

This command looks up for the image name requested in the local, if not available, goes into the docker registry which defaults to docker hub and pull the image and store it into local.

> docker run &lt;image-name&gt;

A single image downloaded might be partitioned into multiple layers based on the size or other factors.

#### Example

> docker run -d --name web -p 80:8080 &lt;image-name&gt;

Where,
* `-d` tell docker to start the container in the background
* `--name` tell it to assign a name to the container being run
* `-p` is telling docker to assign port 8080 running within the docker container to port 80 of the host

### `docker rm` command

To remove a container, you can use this command. To list all the containers quitly with only ids returned, you can use:

> docker ps -aq

Now to remove all the containers, you can use:

> docker rm $(docker ps -aq)

### `docker rmi` command

Get the list of ids of all the images:

> docker image -q

To remove a image, you can use this command. To remove all the images, you can do the following:

> docker rmi $(docker image -q)

## Docker Swarm Mode

Post docker 1.12, we have true native swarm mode(clustering). A swarm aka a cluster, i.e. group of containers.

* A swarm mode consists of multiple containers. 
* `Manager` nodes usually odd numbered 3 or 5, only one is the leader. More managers means more time for consenseus.
* `Worker` nodes execute tasks.

To create a swarm:

> docker swarm init --advertise-addr &lt;MANAGER-IP&gt;

Where,
* `--advertise-addr` flag configures the manager node to publish its address as `MANAGER-IP`.

The above command spits out command to **add a worker** to this swarm:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

To **add a manager** run the following:

> docker swarm join-token manager

To **add a worker** run the following:

> docker swarm join-token worker

### `docker node ls` command

To list down the swarm nodes:

> docker node ls

NOTE: This command can only be run from the manager node. Also, under MANAGER STATUS column only manager nodes has a value, workers has this column as empty. And Leader among them is marked as `Leader`.

### Promote node as manager

> docker node promote &lt;NODE-ID&gt;

## Docker Services

This is introduced to simplify large scale production deployments. A service only runs one image, but controsl what ports it should use, how many replicas of the container should run so the service has the capacity to serve the current load.

Services are declarative in nature. Docker balances the number of service replicas between the **actual state** and **desired state**.

> docker service &lt;create | update | inspect | ls | ps | rm&gt;


To create a service:

> docker service create --name psight -p 8080:8080 --replicas 7 &lt;image-name&gt;

To list all the services created:

> docker service ls

To list all the replicas running:

> docker service ps &lt;service-name&gt;

To scale the number of container running the service:

> docker service scale &lt;service-name&gt;=NUMBER

Eg.

> docker service scale psight=7

OR

> docker service update --replicas 7 psight

## Rolling Updates

It would update the service with the newer version of the image in a incremental manner.