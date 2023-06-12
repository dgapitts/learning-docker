# Problems with using brew for docker setup - Cannot connect to the Docker daemon 

## TLDR

For the newbie there are a surprise number of brew options for a simple docker install

As per the details below none of these worked as ™pure macosx command line setup™ for me!?

The "final solution" for me was to, start docker app via GUI and click on a bunch of end user agreements

##  docker: command not found

```
~/projects/learning-docker $ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
-bash: docker: command not found
```
## brew install docker

Following [brew install docker](https://formulae.brew.sh/formula/docker)

```
~/projects/learning-docker $ brew install docker
Running `brew update --auto-update`...
==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations

==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> New Formulae
ansible@7                         fastgron                          libfastjson                       melange                           spotify_player
apko                              git-credential-oauth              libint                            mods                              swift-outdated
argparse                          git-tools                         libmediainfo                      nexttrace                         tailwindcss-language-server
aws-amplify                       grpc@1.54                         libomemo-c                        openfga                           tern
bashate                           joshuto                           libpaho-mqtt                      procps@3                          votca
core-lightning                    jsign                             lowdown                           protobuf@21                       wzprof
ddns-go                           libecpint                         mariadb@10.11                     shodan                            xbyak
==> New Casks
apple-hewlett-packard-printer-drivers     engine-dj                                 lasso                                     skiff
chatbox                                   eu                                        loupedeck                                 tea
copilot                                   eusamanager                               motu-m-series                             yealink-meeting
craft                                     filemonitor                               mumu-x
devpod                                    firefly-shimmer                           processmonitor
dintch                                    grs-bluewallet                            rode-connect

You have 4 outdated formulae installed.

Warning: Treating docker as a formula. For the cask, use homebrew/cask/docker
==> Fetching docker
==> Downloading https://ghcr.io/v2/homebrew/core/docker/manifests/24.0.2
################################################################################################################################################################# 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/docker/blobs/sha256:8c94c95aaba3da26954bb04ef3c2f74c80a225b8a0646f19c3d77b5d3d69321e
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:8c94c95aaba3da26954bb04ef3c2f74c80a225b8a0646f19c3d77b5d3d69321e?se=2023-06-11T21%3
################################################################################################################################################################# 100.0%
==> Pouring docker--24.0.2.monterey.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/docker/24.0.2: 12 files, 32.4MB
==> Running `brew cleanup docker`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
~/projects/learning-docker $ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.

```

## Cannot connect to the Docker daemon 

```
~/projects/learning-docker $ docker images
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
~/projects/learning-docker $ docker --version
Docker version 24.0.2, build cb74dfcd85
```

## Uninstall docker 

```

~/projects/learning-docker $ brew uninstall docker
Warning: Treating docker as a formula. For the cask, use homebrew/cask/docker
Uninstalling /usr/local/Cellar/docker/24.0.2... (12 files, 32.4MB)
~/projects/learning-docker $ docker --version
-bash: /usr/local/bin/docker: No such file or directory

```

## Install docker via cask

As [recommended on stackoverflow](https://stackoverflow.com/questions/44084846/cannot-connect-to-the-docker-daemon-on-macos)

```
~/projects/learning-docker $ brew install --cask docker
==> Downloading https://raw.githubusercontent.com/Homebrew/homebrew-cask/49c2269a2e26c204a35c00fcf6a393a4aa3c5eb0/Casks/docker.rb
Already downloaded: /Users/dave/Library/Caches/Homebrew/downloads/5005886e60a68675ea8eb4a779afbedfeece8b90d7a428fde8830a821d40f892--docker.rb
==> Downloading https://desktop.docker.com/mac/main/amd64/110738/Docker.dmg
Already downloaded: /Users/dave/Library/Caches/Homebrew/downloads/c9d56831937809c040e5fb41342099c29472c514e24ffa9d04dbbedbcabc3d66--Docker.dmg
==> Installing Cask docker
==> Moving App 'Docker.app' to '/Applications/Docker.app'
==> Linking Binary 'docker' to '/usr/local/bin/docker'
==> Linking Binary 'docker-compose' to '/usr/local/bin/docker-compose'
==> Linking Binary 'docker-compose' to '/usr/local/bin/docker-compose-v1'
==> Linking Binary 'docker-credential-desktop' to '/usr/local/bin/docker-credential-desktop'
==> Linking Binary 'docker-credential-ecr-login' to '/usr/local/bin/docker-credential-ecr-login'
==> Linking Binary 'docker-credential-osxkeychain' to '/usr/local/bin/docker-credential-osxkeychain'
==> Linking Binary 'docker-index' to '/usr/local/bin/docker-index'
==> Linking Binary 'hub-tool' to '/usr/local/bin/hub-tool'
==> Linking Binary 'kubectl' to '/usr/local/bin/kubectl.docker'
==> Linking Binary 'docker.bash-completion' to '/usr/local/etc/bash_completion.d/docker'
==> Linking Binary 'docker-compose.bash-completion' to '/usr/local/etc/bash_completion.d/docker-compose'
==> Linking Binary 'docker.zsh-completion' to '/usr/local/share/zsh/site-functions/_docker'
==> Linking Binary 'docker-compose.zsh-completion' to '/usr/local/share/zsh/site-functions/_docker_compose'
==> Linking Binary 'docker.fish-completion' to '/usr/local/share/fish/vendor_completions.d/docker.fish'
==> Linking Binary 'docker-compose.fish-completion' to '/usr/local/share/fish/vendor_completions.d/docker-compose.fish'
==> Linking Binary 'com.docker.vpnkit' to '/usr/local/bin/vpnkit'
==> Linking Binary 'com.docker.cli' to '/usr/local/bin/com.docker.cli'
🍺  docker was successfully installed!

```


## Same problem


```
~/projects/learning-docker $ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.
```


## Same problem


```
~/projects/learning-docker $ brew services start docker-machine
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Enumerating objects: 2438, done.
remote: Counting objects: 100% (2438/2438), done.
remote: Compressing objects: 100% (1150/1150), done.
remote: Total 2438 (delta 1139), reused 2337 (delta 1100), pack-reused 0
Receiving objects: 100% (2438/2438), 664.79 KiB | 2.93 MiB/s, done.
Resolving deltas: 100% (1139/1139), done.
Tapped 1 command (45 files, 833.1KB).
==> Successfully started `docker-machine` (label: homebrew.mxcl.docker-machine)
```


## Same problem


```
~/projects/learning-docker $ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.
```


## Final solution - start docker app via GUI and click on a bunch of end user agreements


```
~/projects/learning-docker $ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
71d064a1ac7d: Pull complete
Digest: sha256:6e494387c901caf429c1bf77bd92fb82b33a68c0e19f6d1aa6a3ac8d27a7049d
Status: Downloaded newer image for busybox:latest
9b7f7ce1f3fecea63022a6a448d2c5961f073702421261f337d8242e95f03f44
~/projects/learning-docker $  docker pd
docker: 'pd' is not a docker command.
See 'docker --help'
~/projects/learning-docker $  docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS        PORTS     NAMES
9b7f7ce1f3fe   busybox   "sh -c 'while true; …"   10 hours ago   Up 10 hours             test
```