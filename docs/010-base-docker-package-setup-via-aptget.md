## 010-base-docker-package-setup-via-aptget




### Make sure we have a clean starting point - remove any old packages

```
cloud_user@ip-10-0-1-101:~$ sudo apt-get remove docker docker-engine docker.io containerd runc
[sudo] password for cloud_user:
Reading package lists... Done
Building dependency tree
Reading state information... Done
Package 'docker-engine' is not installed, so not removed
Package 'docker' is not installed, so not removed
Package 'containerd' is not installed, so not removed
Package 'docker.io' is not installed, so not removed
Package 'runc' is not installed, so not removed
The following packages were automatically installed and are no longer required:
  linux-aws-5.4-headers-5.4.0-1060 linux-headers-5.4.0-1060-aws linux-image-5.4.0-1060-aws linux-modules-5.4.0-1060-aws
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 26 not upgraded.
cloud_user@ip-10-0-1-101:~$ sudo apt update -y
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic InRelease
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-backports InRelease [83.3 kB]
Get:4 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Get:5 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [3032 kB]
Get:6 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [1908 kB]
Fetched 5201 kB in 1s (3517 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
26 packages can be upgraded. Run 'apt list --upgradable' to see them.
cloud_user@ip-10-0-1-101:~$ sudo apt install -y ca-certificates curl gnupg
Reading package lists... Done
Building dependency tree
Reading state information... Done
ca-certificates is already the newest version (20230311ubuntu0.18.04.1).
curl is already the newest version (7.58.0-2ubuntu3.24).
gnupg is already the newest version (2.2.4-1ubuntu1.6).
The following packages were automatically installed and are no longer required:
  linux-aws-5.4-headers-5.4.0-1060 linux-headers-5.4.0-1060-aws linux-image-5.4.0-1060-aws linux-modules-5.4.0-1060-aws
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 26 not upgraded.
```

### Add the official Docker GPG key

```
cloud_user@ip-10-0-1-101:~$ sudo install -m 0755 -d /etc/apt/keyrings
cloud_user@ip-10-0-1-101:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
gpg: WARNING: unsafe ownership on homedir '/home/cloud_user/.gnupg'
cloud_user@ip-10-0-1-101:~$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Set up the repository
```
cloud_user@ip-10-0-1-101:~$ echo \
> "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
> "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
> sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Update the list of available packages
```
cloud_user@ip-10-0-1-101:~$ sudo apt -y update
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic InRelease
Hit:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-updates InRelease
Hit:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic-backports InRelease
Hit:4 http://security.ubuntu.com/ubuntu bionic-security InRelease
Get:5 https://download.docker.com/linux/ubuntu bionic InRelease [64.4 kB]
Get:6 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages [39.0 kB]
Fetched 103 kB in 0s (253 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
26 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

### Install Docker packages
```
cloud_user@ip-10-0-1-101:~$ sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  linux-aws-5.4-headers-5.4.0-1060 linux-headers-5.4.0-1060-aws linux-image-5.4.0-1060-aws linux-modules-5.4.0-1060-aws
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  docker-ce-rootless-extras libltdl7 pigz
Suggested packages:
  aufs-tools cgroupfs-mount | cgroup-lite
Recommended packages:
  slirp4netns
The following NEW packages will be installed:
  containerd.io docker-buildx-plugin docker-ce docker-ce-cli docker-ce-rootless-extras docker-compose-plugin libltdl7 pigz
0 upgraded, 8 newly installed, 0 to remove and 26 not upgraded.
Need to get 111 MB of archives.
After this operation, 402 MB of additional disk space will be used.
Get:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic/universe amd64 pigz amd64 2.4-1 [57.4 kB]
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu bionic/main amd64 libltdl7 amd64 2.4.6-2 [38.8 kB]
Get:3 https://download.docker.com/linux/ubuntu bionic/stable amd64 containerd.io amd64 1.6.21-1 [28.3 MB]
Get:4 https://download.docker.com/linux/ubuntu bionic/stable amd64 docker-buildx-plugin amd64 0.10.5-1~ubuntu.18.04~bionic [26.1 MB]
Get:5 https://download.docker.com/linux/ubuntu bionic/stable amd64 docker-ce-cli amd64 5:24.0.2-1~ubuntu.18.04~bionic [13.3 MB]
Get:6 https://download.docker.com/linux/ubuntu bionic/stable amd64 docker-ce amd64 5:24.0.2-1~ubuntu.18.04~bionic [22.9 MB]
Get:7 https://download.docker.com/linux/ubuntu bionic/stable amd64 docker-ce-rootless-extras amd64 5:24.0.2-1~ubuntu.18.04~bionic [9014 kB]
Get:8 https://download.docker.com/linux/ubuntu bionic/stable amd64 docker-compose-plugin amd64 2.18.1-1~ubuntu.18.04~bionic [10.9 MB]
Fetched 111 MB in 2s (47.8 MB/s)
Selecting previously unselected package pigz.
(Reading database ... 149063 files and directories currently installed.)
Preparing to unpack .../0-pigz_2.4-1_amd64.deb ...
Unpacking pigz (2.4-1) ...
Selecting previously unselected package containerd.io.
Preparing to unpack .../1-containerd.io_1.6.21-1_amd64.deb ...
Unpacking containerd.io (1.6.21-1) ...
Selecting previously unselected package docker-buildx-plugin.
Preparing to unpack .../2-docker-buildx-plugin_0.10.5-1~ubuntu.18.04~bionic_amd64.deb ...
Unpacking docker-buildx-plugin (0.10.5-1~ubuntu.18.04~bionic) ...
Selecting previously unselected package docker-ce-cli.
Preparing to unpack .../3-docker-ce-cli_5%3a24.0.2-1~ubuntu.18.04~bionic_amd64.deb ...
Unpacking docker-ce-cli (5:24.0.2-1~ubuntu.18.04~bionic) ...
Selecting previously unselected package docker-ce.
Preparing to unpack .../4-docker-ce_5%3a24.0.2-1~ubuntu.18.04~bionic_amd64.deb ...
Unpacking docker-ce (5:24.0.2-1~ubuntu.18.04~bionic) ...
Selecting previously unselected package docker-ce-rootless-extras.
Preparing to unpack .../5-docker-ce-rootless-extras_5%3a24.0.2-1~ubuntu.18.04~bionic_amd64.deb ...
Unpacking docker-ce-rootless-extras (5:24.0.2-1~ubuntu.18.04~bionic) ...
Selecting previously unselected package docker-compose-plugin.
Preparing to unpack .../6-docker-compose-plugin_2.18.1-1~ubuntu.18.04~bionic_amd64.deb ...
Unpacking docker-compose-plugin (2.18.1-1~ubuntu.18.04~bionic) ...
Selecting previously unselected package libltdl7:amd64.
Preparing to unpack .../7-libltdl7_2.4.6-2_amd64.deb ...
Unpacking libltdl7:amd64 (2.4.6-2) ...
Setting up containerd.io (1.6.21-1) ...
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /lib/systemd/system/containerd.service.
Setting up docker-ce-rootless-extras (5:24.0.2-1~ubuntu.18.04~bionic) ...
Setting up docker-buildx-plugin (0.10.5-1~ubuntu.18.04~bionic) ...
Setting up libltdl7:amd64 (2.4.6-2) ...
Setting up docker-compose-plugin (2.18.1-1~ubuntu.18.04~bionic) ...
Setting up docker-ce-cli (5:24.0.2-1~ubuntu.18.04~bionic) ...
Setting up pigz (2.4-1) ...
Setting up docker-ce (5:24.0.2-1~ubuntu.18.04~bionic) ...
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
Processing triggers for libc-bin (2.27-3ubuntu1.6) ...
Processing triggers for systemd (237-3ubuntu10.57) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Processing triggers for ureadahead (0.100.0-21) ...
```






