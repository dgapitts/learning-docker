# 020-hello-docker-world-and-removing-sudo-requirement




## xxx

```
cloud_user@ip-10-0-1-101:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:

 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

## Make sure the docker group was created

```
cloud_user@ip-10-0-1-101:~$ sudo groupadd docker
groupadd: group 'docker' already exists
```

## Add currrent `$USER` to `docker` group

```
cloud_user@ip-10-0-1-101:~$ sudo usermod -aG docker $USER
cloud_user@ip-10-0-1-101:~$ id
uid=1001(cloud_user) gid=1001(cloud_user) groups=1001(cloud_user),27(sudo),108(lxd)
```

## Either log out and back in as the cloud_user, or run the `newgrp` command:

```
cloud_user@ip-10-0-1-101:~$ newgrp docker
cloud_user@ip-10-0-1-101:~$ id
uid=1001(cloud_user) gid=999(docker) groups=999(docker),27(sudo),108(lxd),1001(cloud_user)
```

## We can rerun without sudo now

```
cloud_user@ip-10-0-1-101:~$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
