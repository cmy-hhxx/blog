# docker 教程
## 关键在于如何使用man来学习docker
```
man docker
docker -h
man docker COMMAND
docker COMMAND --help
```
## man docker
```text
The Docker CLI has over 30 commands.
The commands are listed below and each has its own man page which explain usage and arguments.
docker option
```

## docker -h
```
# Management Commands

1. image
2. manifest
3. network
4. node
5. service
6. system
7. volume

# Commands

1. attach
2. build
3. commit
4. create
5. diff
6. exec
7. export
8. history
9. images
10. info
11. kill
12. load
13. port
14. pull
15. push
16. rename
17. restart
18. rm
19. rmi
20. run
21. save
22. search
23. start
24. starts
25. stop
26. tag
27. top
```


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
