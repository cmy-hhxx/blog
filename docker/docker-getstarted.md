# docker 教程
## 关键在于如何使用man来学习docker

```
man docker
docker -h
man docker COMMAND
docker COMMAND --help
```

---

## man docker
```text
The Docker CLI has over 30 commands.
The commands are listed below and each has its own man page which explain usage and arguments.
docker option
```

---

## docker -h
```text
# Management Commands

1. image

2. manifest
docker manifest inspect, display an image manifest or manifest list.
synopsis = docker manifest inspect [OPTIONS] MANIFEST

3. network

4. node

5. service

6. system
docker system df, show docker disk usage.
synopsis = docker system df [OPTIONS]

docker system events, get real time events from the server.
synopsis = docker system events [OPTIONS]

docker system info, display system-wide information.
synopsis = docker system info [OPTIONS]

docker system prune, remove unused data.
synopsis = docker system prune

7. volume
docker volumes create, create a volume.
synopsis = docker volumes create [OPTIONS] [VOLUME]

docker volumes inspect, display detailed information on one or more volumes.
synopsis = docker volume inspect [OPTIONS] VOLUME [VOLUME...]

docker volumes ls
docker volumes prune
docker volumes rm

# Commands

1. attach
docker attach, attach standard input, output, and error streams to a running container.
synopsis = docker attach [OPTIONS] CONTAINER

2. build
docker build, build an image from a Dockerfile.
synopsis = docker build [OPTIONS] PATH | URL | -

3. commit
docker commit, create a new image from a container's changes.
synopsis = docker commit [OPTIONS] CONTAINER [REPOOSITORY[:TAG]]

4. create
docker create, create a new container.
synopsis = docker create [OPTIONS] IMAGE [COMMAND] [ARG...]

5. diff
docker diff, inspect changes to files or directories on container's filesystem.
synopsis = docker diff CONTAINER

6. exec
docker exec, run a command in a running container.
synopsis = docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

7. export
docker export, export a container's filesystem as a tar arhive.
synopsis = docker export [OPTIONS] CONTAINER

8. history
docker history, show the history of an image.
synopsis = docker history [OPTIONS] IMAGE
9. images

10. info
docker info, display system-wide information.
synopsis = docker info [OPTIONS]

11. kill
docker kill, kill one or more running containers.
synopsis = docker kill [OPTIONS] CONTAINER [CONTAINER...]

12. load
docker load, load an image from a tar archive or STDIN.
synopsis = docker load [OPTIONS]

13. port
docker port, list port mappings or a specific mapping for the container.
synopsis = docker port CONTAINER [PRIVATE_PORT[/PROTO]]

14. pull
docker pull = docker image pull

15. push
docker push, push an image or a repository to a registry.
synopsis = docker push [OPTIONS] NAME[:TAG]

16. rename
docker rename, rename a container.
synopsis = docker rename CONTAINER NEW_NAME

17. restart
docker restart, restart one or more containers.
synopsis = docker restart [OPTIONS] CONTAINER [CONTAINER...]

18. rm
docker rm, remove one or more containers.
synopsis = docker rm [OPTINS] CONTAINER [CONTAINER...]

19. rmi
docker rmi = docker image rm

20. run
see man docker-run below

21. save
docker save = docker image save

22. search
docker search, search the Docker Hub for images.
synopsis = docker search [OPTIONS] TERM

23. start
docker start, start one or more stopped containers.
synopsis = docker start [OPTIONS] CONTAINER [CONTAINER...]

24. stats
docker stats, display a live stream of container resource usage statistics.
synopsis = docker stats [OPTIONS] [CONTAINER...]

25. stop
docker stop, stop one or more running containers.
synopsis = docker stop [OPTIONS] CONTAINER [CONTAINER...]

26. tag
docker tag = docker image tag

27. top
docker top, display the running process of a container.
synopsis = docker top CONTAINER [ps CONTAINER]

```

---

## docker image -h
```text
Commands
1. docker image build, build an image from a Dockerfile.
Alias = docker build
synopsis = docker image build [OPTIONS] PATH | URL | -

2. docker image history, show the history of an image
synopsis = docker image history [OPTIONS] IMAGE

3. docker image import, import the content from a tarball to create a filesystem image
synopsis = docker image import [OPTIONS] file | URL

4. docker image inspect, display the detailed information on one or more images
synopsis = docker image inspect [OPTIONS] IMAGE [IMAGE...]

5. docker image load, load an image from a tar archive or STDIN
synopsis = docker image load

6. docker image prune, remove unused images
synopsis = docker image prune [OPTIONS]

7. docker image pull, pull an image or a repository from a registry.
synopsis = docker image pull [OPTIONS] NAME[:TAG|@DIGEST]

8. docker image rm, remove one or more images
synopsis = docker image rm [OPTIONS] IMAGE [IMAGE...]

9. docker image save, save one or more images to a tar archive.
synopsis = docker image save [OPTIONS] IMAGE [IMAGE...]

10. docker image tag, create a target_image that refers to source_image
synopsis = docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

```

---

## man docker-network
```text
1. docker network connect, connect a container to a network.
synopsis = docker network connect [OPTIONS] NETWORK CONTAINER

2. docker network create, create a network.
synopsis = docker network create [OPTIONS] NETWORK

3. docker network disconnect, disconnect a container from a network.
synopsis = docker network disconnect [OPTIONS] NETWORK CONTAINER

4. docker network inspect, display detailed information on one or more networks
synopsis = docker network inspect [OPTIONS] NETWORK [NETWORK...]

5. docker network prune, remove all unused networks.
synopsis = docker network prune [OPTIONS]

6. docker network rm, remove one or more networks.
synopsis = docker network rm NETWORK [NETWORKS]

```

---

## man docker-run
```text
DECSRIPTIONS：
1. Run a process in a new container.
2. A process with its own neworking, its own filesystem, its own process tree.
3. If the image is not found, docker run will first pull the image, then start the image.

OPTIONS:
1. -d, run the container in the background and print the new container ID.
When attached in the tty mode, you can detach from the container (and leave it running) using a configurable key sequence.
The default sequence is CTRL-p CTRL-q.

2. -h, sets the container host name that is available inside the container.

3. -i, Keep STDIN open even if not attached. The default is false.
When set to true, keep stdin open even if not attached.

4. --name, assign a name to the container.
IDENTIFIER TYPE = UUID long || UUID short || name

5. --network, set the Network mode for the container

6. -P, publish all exposed ports to random ports on the host interfaces.
The default is false.

7. -p, publish a container's port, or range of ports, to the host.
Both hostPort and containerPort can be specified as a range.
When specifying ranges for both, the number of ports in ranges should be equal.

8. --rm, automatically remove the container when it exits. The default is false.

9. -t, allocate a pseudo-TTY(teletypewriters). The default is false.
```
