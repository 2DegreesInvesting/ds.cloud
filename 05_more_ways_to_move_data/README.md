# More ways to move data around

### Setup

Connect your local and remote computers via SSH: [account/security > Add SSH Key](https://cloud.digitalocean.com/account/security)

Local terminal (e.g. your laptop)

```bash
# Add some data
mkdir /tmp/data
cd /tmp
touch data/a.csv data/b.csv
ls
```

Remote terminal (e.g. a DigitalOcean droplet)

```bash
# Connect to the server
ssh root@174.138.4.109
```

#### Move data between your local and remote computers with `scp`

Copy a directory from local to remote computer:

```bash
# Local terminal
scp -r data root@174.138.4.109:/tmp
```

```bash
# Remote terminal
ls /tmp/data
```

Copy directory from a remote host to local host:

```bash
# Local terminal
scp -r root@174.138.4.109:/tmp/data ~/Downloads
ls ~/Downloads/data
```


#### Move data between a server and a container

> Volumes are the preferred mechanism for persisting data generated by and used
by Docker containers. While **bind mounts** are dependent on the directory
structure and OS of the host machine, **volumes** are completely managed by
Docker.  
-- https://docs.docker.com/storage/volumes/

```bash
# Connect to the server
ssh root@174.138.4.109
```

```bash
# Server's terminal

# Run a container from https://www.rocker-project.org/
docker run --rm -d --name my_container \
  -v volume_managed_by_docker:/home/rstudio/volume_managed_by_docker \
  -v /bind_mount:/home/rstudio/bind_mount \
  -p 8787:8787 -e PASSWORD=123 \
  -e ROOT=true \
  rocker/verse
```

```bash
# Server's terminal

# `volume_managed_by_docker` is more portable
# It does not depend on the host's file system
docker volume ls

# If it doesn't exist yet, it's created
ls /bind_mount
```

```bash
# Container's terminal: http://174.138.4.109:8787

# `/bind_mount` is less portable
# It depends on the host's file system
ls /

# Fails
touch bind_mount/a.csv
# Works but tedious
sudo touch bind_mount/a.csv

# More general
sudo chown -R rstudio:rstudio /home/rstudio

# Works everywhere under ~
touch bind_mount/a.csv
touch volume_managed_by_docker/b.csv
```

#### Move data between containers

```bash
# Server's terminal

# Another container with access to the data in `volume_managed_by_docker`
docker run --rm -ti -v volume_managed_by_docker:/volume_managed_by_docker bash

# Terminal in the new container
ls /volume_managed_by_docker
```

### Resources

* [How to setup SSH keys](https://www.digitalocean.com/community/tutorial_collections/how-to-set-up-ssh-keys), or go to <https://cloud.digitalocean.com/account/security> then "Add SSH Key".

* [`scp` explained](https://phoenixnap.com/kb/linux-scp-command)
