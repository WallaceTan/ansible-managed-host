# Ansible installation on Centos 7 base image

## Generate SSH keys for ansible user
```
ssh-keygen -t rsa -b 2048 -C "ansible" -f .ssh/id_rsa
```

## Build docker base images
```
docker build -t ansible-centos -f Dockerfile-ansible .
docker build -t managed-host-centos -f Dockerfile-managed-host .
```

## Start docker containers
```
docker-compose up -d
```

## View all running docker containers
```
$ docker ps
CONTAINER ID        IMAGE                        COMMAND               CREATED             STATUS              PORTS               NAMES
1c3b4ed95413        ansible-centos:latest        "tail -f /dev/null"   4 seconds ago       Up 3 seconds                            ansible-control
33a8a512c316        managed-host-centos:latest   "/usr/sbin/sshd -D"   6 seconds ago       Up 5 seconds        22/tcp              web
bfbf3ee3c999        managed-host-centos:latest   "/usr/sbin/sshd -D"   6 seconds ago       Up 5 seconds        22/tcp              app1
6717d91194b9        managed-host-centos:latest   "/usr/sbin/sshd -D"   6 seconds ago       Up 4 seconds        22/tcp              app2
7eba120c6448        managed-host-centos:latest   "/usr/sbin/sshd -D"   6 seconds ago       Up 5 seconds        22/tcp              db
```

## Connect to ansible-control docker container
```
docker exec -it ansible-control /bin/bash
```

## Test ansible in ansible-control container
```
ansible --version
```

```
ansible docker-hosts -m ping
```

```
ansible docker-hosts -m command -a "ping -c2 8.8.8.8"
```

```
ansible docker-hosts -m command -a "curl -s http://www.google.com"
```
