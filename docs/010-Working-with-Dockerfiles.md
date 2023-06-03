### 010 Working with Dockerfiles

Starting with a simple Dockerfile, building `FROM httpd:2.4`add some extra package statements via àpt`
```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ vim Dockerfile
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ cat Dockerfile
FROM httpd:2.4

RUN apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists*
```
### docker build with tag widgetfactory:0.1

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker build -t widgetfactory:0.1 .
[+] Building 6.4s (6/6) FINISHED
 => [internal] load build definition from Dockerfile                                                                     0.0s
 => => transferring dockerfile: 213B                                                                                     0.0s
 => [internal] load .dockerignore                                                                                        0.0s
 => => transferring context: 2B                                                                                          0.0s
 => [internal] load metadata for docker.io/library/httpd:2.4                                                             0.0s
 => [1/2] FROM docker.io/library/httpd:2.4                                                                               0.1s
 => [2/2] RUN apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists*            6.2s
 => exporting to image                                                                                                   0.1s
 => => exporting layers                                                                                                  0.1s
 => => writing image sha256:89a5bd0738657c7b3c0d92180990d0396e8846930df14413e801d1ac3a5e3cba                             0.0s
 => => naming to docker.io/library/widgetfactory:0.1                                                                     0.0s
 ```
### export custom showLayers and showSize

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ export showLayers='{{ range .RootFS.Layers }}{{ println . }}{{end}}'
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ export showSize='{{ .Size }}'
```
### docker images

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
widgetfactory   0.1       89a5bd073865   35 seconds ago   152MB
httpd           2.4       d1676199e605   10 days ago      145MB
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showSize" widgetfactory:0.1
151938544
```
### docker inspect - widgetfactory:0.1 

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" widgetfactory:0.1
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
sha256:e445b513b4b88ab2af6b24c0b930cea4906d82ecc102903f29a3343a86ce75d7

[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" httpd:2.4
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
```
### docker build with tag widgetfactory:0.1

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ vim Dockerfile
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ cat Dockerfile
FROM httpd:2.4

RUN apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists*
RUN rm -f /usr/local/apache2/htdocs/index.html
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker build -t widgetfactory:0.2 .
[+] Building 0.4s (7/7) FINISHED
 => [internal] load .dockerignore                                                                                        0.0s
 => => transferring context: 2B                                                                                          0.0s
 => [internal] load build definition from Dockerfile                                                                     0.0s
 => => transferring dockerfile: 261B                                                                                     0.0s
 => [internal] load metadata for docker.io/library/httpd:2.4                                                             0.0s
 => [1/3] FROM docker.io/library/httpd:2.4                                                                               0.0s
 => CACHED [2/3] RUN apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists*     0.0s
 => [3/3] RUN rm -f /usr/local/apache2/htdocs/index.html                                                                 0.3s
 => exporting to image                                                                                                   0.0s
 => => exporting layers                                                                                                  0.0s
 => => writing image sha256:b64e4ae36f0e6b98b6ae6eaee6685b907a9a482d9ec3890ff1bc8dfaa0039431                             0.0s
 => => naming to docker.io/library/widgetfactory:0.2 
```
### docker inspect - widgetfactory:0.2

```                                                                    0.0s
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" widgetfactory:0.1
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
sha256:e445b513b4b88ab2af6b24c0b930cea4906d82ecc102903f29a3343a86ce75d7

[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" widgetfactory:0.2
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
sha256:e445b513b4b88ab2af6b24c0b930cea4906d82ecc102903f29a3343a86ce75d7
sha256:890d3fb5f3b8489e42d2576e258ff0002926e268e12861cf89b3618c51d66863
```
### docker run widgetfactory:0.2 - bash shell

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker run --rm -it widgetfactory:0.2 bash
root@275a1a8868bf:/usr/local/apache2# ls htdocs/
root@275a1a8868bf:/usr/local/apache2# exit
exit
```
### docker build with tag widgetfactory:0.1

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ vim Dockerfile
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker build -t widgetfactory:0.3 .
[+] Building 0.2s (10/10) FINISHED
 => [internal] load .dockerignore                                                                                        0.0s
 => => transferring context: 2B                                                                                          0.0s
 => [internal] load build definition from Dockerfile                                                                     0.0s
 => => transferring dockerfile: 308B                                                                                     0.0s
 => [internal] load metadata for docker.io/library/httpd:2.4                                                             0.0s
 => [1/5] FROM docker.io/library/httpd:2.4                                                                               0.0s
 => [internal] load build context                                                                                        0.0s
 => => transferring context: 22.46kB                                                                                     0.0s
 => CACHED [2/5] RUN apt update -y && apt upgrade -y && apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists*     0.0s
 => CACHED [3/5] RUN rm -f /usr/local/apache2/htdocs/index.html                                                          0.0s
 => [4/5] WORKDIR /usr/local/apache2/htdocs                                                                              0.0s
 => [5/5] COPY ./web .                                                                                                   0.0s
 => exporting to image                                                                                                   0.0s
 => => exporting layers                                                                                                  0.0s
 => => writing image sha256:e1931728d7aa116062ebbc0c19d08b63ab48093f6c84becc685670e4ee04be39                             0.0s
 => => naming to docker.io/library/widgetfactory:0.3                                                                     0.0s
```
### docker inspect - widgetfactory:0.3 

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" widgetfactory:0.2
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
sha256:e445b513b4b88ab2af6b24c0b930cea4906d82ecc102903f29a3343a86ce75d7
sha256:890d3fb5f3b8489e42d2576e258ff0002926e268e12861cf89b3618c51d66863

[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker inspect -f "$showLayers" widgetfactory:0.3
sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
sha256:eef322f11a380e94c7e87aa98a9803d3cadf739ebc036c9d4c1028c6efc2f9d2
sha256:c61a2444e1002135dc228c44df5ffd02bf2d0c93f92167ab6a28463e8fb561ff
sha256:19a01b7b25679fc82106dec3489f7211a51266977bf65fef99f1916b8bef3b76
sha256:678572996b1dafe1b0bb4d16dcf8f9460716b037c7912c6e2d0efeb8421553a9
sha256:e445b513b4b88ab2af6b24c0b930cea4906d82ecc102903f29a3343a86ce75d7
sha256:890d3fb5f3b8489e42d2576e258ff0002926e268e12861cf89b3618c51d66863
sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef
sha256:e93f40f3176431c8867517c0831255f0a4305e99f3bd051d11d6bcd5629c5b38
```
### docker run widgetfactory:0.3 - bash shell

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker run --rm -it widgetfactory:0.3 bash
root@15b43b402d24:/usr/local/apache2/htdocs# ls -l
total 16
drwxr-xr-x. 2 root root   76 Jun  2 19:24 img
-rw-r--r--. 1 root root 3059 Jun  2 19:24 index.html
-rw-r--r--. 1 root root 2910 Jun  2 19:24 quote.html
-rw-r--r--. 1 root root 2611 Jun  2 19:24 support.html
-rw-r--r--. 1 root root 2645 Jun  2 19:24 widgets.html
root@15b43b402d24:/usr/local/apache2/htdocs# docker run --name web1 -p 80:80 widgetfactory:0.3
bash: docker: command not found
root@15b43b402d24:/usr/local/apache2/htdocs# exit
exit
```
###  docker run web1

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker run --name web1 -p 80:80 widgetfactory:0.3
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName'directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName'directive globally to suppress this message
[Fri Jun 02 21:00:18.842888 2023] [mpm_event:notice] [pid 1:tid 139974189104448] AH00489: Apache/2.4.57 (Unix) configured -- resuming normal operations
[Fri Jun 02 21:00:18.843166 2023] [core:notice] [pid 1:tid 139974189104448] AH00094: Command line: 'httpd -D FOREGROUND'
^C[Fri Jun 02 21:00:24.069536 2023] [mpm_event:notice] [pid 1:tid 139974189104448] AH00491: caught SIGTERM, shutting down
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker ps -a
CONTAINER ID   IMAGE               COMMAND              CREATED          STATUS                      PORTS     NAMES
b61f36dd3312   widgetfactory:0.3   "httpd-foreground"   18 seconds ago   Exited (0) 12 seconds ago             web1
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker ps -a
CONTAINER ID   IMAGE               COMMAND              CREATED          STATUS                      PORTS     NAMES
b61f36dd3312   widgetfactory:0.3   "httpd-foreground"   24 seconds ago   Exited (0) 18 seconds ago             web1
```
###  docker start web1

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker start web1
web1
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker ps -a
CONTAINER ID   IMAGE               COMMAND              CREATED          STATUS         PORTS                               NAMES
b61f36dd3312   widgetfactory:0.3   "httpd-foreground"   39 seconds ago   Up 2 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   web1
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ docker exec -it web1 bash
root@b61f36dd3312:/usr/local/apache2/htdocs# ls -l
total 16
drwxr-xr-x. 2 root root   76 Jun  2 19:24 img
-rw-r--r--. 1 root root 3059 Jun  2 19:24 index.html
-rw-r--r--. 1 root root 2910 Jun  2 19:24 quote.html
-rw-r--r--. 1 root root 2611 Jun  2 19:24 support.html
-rw-r--r--. 1 root root 2645 Jun  2 19:24 widgets.html
root@b61f36dd3312:/usr/local/apache2/htdocs# exit
exit
```
### wget localhost - connecting to httpd server in docker container

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ wget localhost
--2023-06-02 17:01:41--  http://localhost/
Resolving localhost (localhost)... ::1, 127.0.0.1
Connecting to localhost (localhost)|::1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3059 (3.0K) [text/html]
Saving to: ‘index.html’

100%[====================================================================================>] 3,059       --.-K/s   in 0s

2023-06-02 17:01:41 (225 MB/s) - ‘index.html’ saved [3059/3059]

[cloud_user@ip-10-0-1-229 widget-factory-inc]$ diff index.html web/index.html
```
### Finally review the underlying/backgroumd linuxacademy/content-widget-factory-inc project

```
[cloud_user@ip-10-0-1-229 widget-factory-inc]$ cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://github.com/linuxacademy/content-widget-factory-inc.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[cloud_user@ip-10-0-1-229 widget-factory-inc]

