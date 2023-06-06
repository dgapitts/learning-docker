# 030-Set-Default-Logging-Driver-to-syslog

## Edit daemon.json

```
sudo vi /etc/docker/daemon.json
```

Add configuration to daemon.json to set the default logging driver:

```
{
  "log-driver": "syslog"
}
```
...
```
cloud_user@ip-10-0-1-101:~$ sudo vi /etc/docker/daemon.json
cloud_user@ip-10-0-1-101:~$ cat /etc/docker/daemon.json
{
          "log-driver": "syslog"
}
```


## Restart Docker

```
sudo systemctl restart docker
```

Verify that the logging driver was set properly:

```
docker info | grep Logging
```

This command should return a line that says:

```
Logging Driver: syslog
```
...
```
cloud_user@ip-10-0-1-101:~$ docker info | grep Logging
 Logging Driver: syslog
WARNING: No swap limit support
```

## Configure Docker to start on boot:

```
sudo systemctl enable docker.service
```
...
```
cloud_user@ip-10-0-1-101:~$ sudo systemctl enable docker.service
Synchronizing state of docker.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable docker
```

Configure the containerd service to start on boot:
```
sudo systemctl enable containerd.service
```



