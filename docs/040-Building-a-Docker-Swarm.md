# 040-Building-a-Docker-Swarm

> Docker swarm allows you to quickly move beyond simply using Docker to run containers. With swarm, you can easily set up a cluster of Docker servers capable of providing useful orchestration features


## manager node `docker swarm init`


Setup master i.e. docker version:
```
cloud_user@ip-10-0-1-101:~$ docker version
Client:
 Version:           18.09.5
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        e8ff056
 Built:             Thu Apr 11 04:43:57 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.5
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       e8ff056
  Built:            Thu Apr 11 04:10:53 2019
  OS/Arch:          linux/amd64
  Experimental:     false

```


On the swarm manager server, initialize the swarm. Be sure to replace <swarm manager private IP> in this command with the actual Private IP of the swarm manager (NOT the public IP).

```
docker swarm init --advertise-addr <swarm manager private IP>
```

and in the lab, interesting the private IP address is in the host name which is part of PS1 (the custom prompt)
```
cloud_user@ip-10-0-1-101:~$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:7d:e0:85:b4  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens5: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 10.0.1.101  netmask 255.255.255.0  broadcast 10.0.1.255
        inet6 fe80::8fb:2bff:fefa:b9e9  prefixlen 64  scopeid 0x20<link>
        ether 0a:fb:2b:fa:b9:e9  txqueuelen 1000  (Ethernet)
        RX packets 221684  bytes 324389559 (324.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 24606  bytes 1754582 (1.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 376  bytes 37954 (37.9 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 376  bytes 37954 (37.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
and setting up the master node
```
cloud_user@ip-10-0-1-101:~$ docker swarm init --advertise-addr  10.0.1.101
Swarm initialized: current node (qynp7zkexmbs6ehmh5o6kw80h) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0mpxiwqbhwzj752wzb732ugx4zsaxdvi4ew2pvl5w1hl3xkobb-dc9neomdlcnihxt0430ropq13 10.0.1.101:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```


## work node initialization `docker swarm join`

Setup worker node with the same docker version:
```
cloud_user@ip-10-0-1-102:~$ docker version
Client:
 Version:           18.09.5
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        e8ff056
 Built:             Thu Apr 11 04:43:57 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.5
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       e8ff056
  Built:            Thu Apr 11 04:10:53 2019
  OS/Arch:          linux/amd64
  Experimental:     false

```
 
Using the dynamically generated command-token from the manager node initialization
```
docker swarm join --token SWMTKN-1-0mpxiwqbhwzj752wzb732ugx4zsaxdvi4ew2pvl5w1hl3xkobb-dc9neomdlcnihxt0430ropq13 10.0.1.101:2377
```
on the worker node we want to add
```
cloud_user@ip-10-0-1-102:~$ docker swarm join --token SWMTKN-1-0mpxiwqbhwzj752wzb732ugx4zsaxdvi4ew2pvl5w1hl3xkobb-dc9neomdlcnihxt0430ropq13 10.0.1.101:2377
This node joined a swarm as a worker.
cloud_user@ip-10-0-1-102:~$ docker node ls
Error response from daemon: This node is not a swarm manager. Worker nodes can't be used to view or modify cluster state. Please run this command on a manager node or promote the current node to a manager.

```

## manager node `docker node ls` ... cluster status

```
cloud_user@ip-10-0-1-101:~$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
qynp7zkexmbs6ehmh5o6kw80h *   ip-10-0-1-101       Ready               Active              Leader              18.09.5
h48gsu4c3nzr4pc8846fpy9al     ip-10-0-1-102       Ready               Active                                  18.09.5

```

## worker node `docker node ls` ... cluster error

> Worker nodes can't be used to view or modify cluster state

```
cloud_user@ip-10-0-1-102:~$ docker node ls
Error response from daemon: This node is not a swarm manager. Worker nodes can't be used to view or modify cluster state. Please run this command on a manager node or promote the current node to a manager.
```
