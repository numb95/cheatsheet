# Docker

#### Install

###### Ubuntu

```
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

$ sudo apt update

$ sudo apt install docker-ce
```

###### CentOS

```
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

$ sudo yum install docker-ce
```

#### Manage Docker as a non-root user

1. Create the `docker` group.

   `$ sudo groupadd docker`

2. Add your user to the `docker` group.

   `$ sudo usermod -aG docker $USER`

3. Log out and log back in so that your group membership is re-evaluated.

### Build

Build an image from the Dockerfile in the current directory and tag the image:

`docker build -t myapp:1.0 .`

List all images that are locally stored with the Docker engine:

`docker images`

Delete an image from the local image store:

`docker rmi alpine:3.4`

### Ship

Pull an image from a registry:

`docker pull alpine:3.4`

Retag a local image with a new image name and tag:

`docker tag alpine:3.4 myrepo/myalpine:3.4`

Log in to a registry (the Docker Hub by default):

`docker login my.registry.com:8000`

Push an image to a registry:

`docker push myrepo/myalpine:3.4`

### Run

```
docker run --rm -it
    --name web
    -p 5000:80
    -v ~/dev:/code
    alpine:3.4 /bin/sh
```

- `--rm` remove container automatically after it exits
- `-it` connect the container to terminal
- `--name web` name the container
- `-p 5000:80` expose port 5000 externally and map to port 80
- `-v ~/dev:/code` create a host mapped volume inside the container
- `alpine:3.4` the image from which the container is instantiated
- `/bin/sh` the command to run inside the container

Stop a running container through SIGTERM:

`docker stop web`

Stop a running container through SIGKILL:

`docker kill web`

Create an overlay network and specify a subnet:

`docker network create --subnet 10.1.0.0/24 --gateway  10.1.0.1 -d overlay mynet`

List the networks:

`docker network ls`

List the running containers:

`docker ps`

Delete all running and stopped containers:

`docker rm -f $(docker ps -aq)`

Create a new bash process inside the container and connect it to the terminal:

`docker exec -it web bash`

Print the last 100 lines of a container’s logs:

`docker logs --tail 100 web`