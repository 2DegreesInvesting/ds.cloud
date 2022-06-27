### Increase disk space to store more data

> As an ananlyst I would like to increase my disk space as my data grows.

Resize: 

* Note resize options under "Storage-Optimized" and "Disk, CPU and RAM"

> "This will increase the disk size, CPU and RAM of your Droplet. This is a
permanent change and cannot be reversed."

Volumes:

* Add a volume. [Volumes cost $0.10 GiB per
month](https://docs.digitalocean.com/products/volumes/details/pricing/).

* See information about the droplet's file system: `df -h`.

* Find the volume: `ls /mnt`

* Note you can increase the size (Volumes > More).

Resources:

* <https://docs.digitalocean.com/products/volumes/quickstart/>

### Share data across team members with pins

> As an analyst working in a team I would like to have a way to share data
better than Dropbox.

Each team member may use a different droplet or container, yet share data in a
volume easily with [pins](https://pins.rstudio.com/).

* The cloud manager runs creates a folder for shared/ data and pins/:

```bash
# Create a dedicated folder for pins
mkdir /mnt/volume/shared

# Share it with everyone: Add read/write/execute permission for all
chmod a+rwx /mnt/volume/shared
ll /mnt/volume
```

* The cloud manager creates a server for each team member:

```bash
# Linda's container on port 8787
docker run -d \
  --name linda -p 8787:8787 -e PASSWORD=123 \
  -v /mnt/volume/shared:/mnt/volume/shared \
  -e ROOT=true \
  rocker/verse

# Mauro's container on port 8788
docker run -d \
  --name mauro -p 8788:8787 -e PASSWORD=321 \
  -v /mnt/volume/shared:/mnt/volume/shared \
  -e ROOT=true \
  rocker/verse

# Show running containers
docker ps
```

* The analysts login at `http://ipv4:their.port`  with root "rstudio" and
their password, then share data with pins.

```r
# Linda writes some data
# install.packages("pins")
library(pins)

board <- board_folder("/mnt/volume/shared/pins")
board |> pin_write(mtcars, "mtcars")
board |> pin_list()
```

```r
# Mauro uses the data that Linda created
# install.packages("pins")
library(pins)

board <- board_folder("/mnt/volume/shared/pins")
board |> pin_list()
board |> pin_read("mtcars")
```

Resources:

* <https://pins.rstudio.com/>.
* <https://github.com/2DegreesInvesting/ds.pins>.
* [Share volume across
droplets](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-18-04).

### Move data around

Setup for an example droplet at `174.138.4.109` (your ipv4 will be different).

```bash
# Connect to the droplet
ssh root@174.138.4.109

# Run a container from https://www.rocker-project.org/
docker run --rm -d \
  -p 8787:8787 -e PASSWORD=123 \
  -e ROOT=true \
  rocker/verse

# Login to rstudio at http://174.138.4.109:8787

# Tell Git who you are, or example
git config --global user.email "maurolepore@gmail.com"
git config --global user.name "Mauro Lepore"
```

#### Move data between a container (or any server) and GitHub with `gh`

Setup the [`gh` CLI](https://cli.github.com/).

```bash
# https://github.com/cli/cli#installation, e.g. for Linux
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Authenticate with GitHub.com via a new SSH key
gh auth login
```

Get and share small datasets as usual.

```bash
# Or `git clone git@github.com:2DegreesInvesting/ds.cloud.git`
gh repo clone 2degreesinvesting/ds.cloud

# Add some data
mkdir data
touch data/a.csv data/b.csv

# Share data as usual
git add data
git commit -m "New data"
git push
```

Share bigger datasets as assets attached to GitHub releases.

```bash
# Create a GitHub release
gh release craete v1

# Create (c) a compressed (z) file (f) named "data.tar.gz" from files in "data/"
tar czf data.tar.gz data

# Upload the data and attach it as an asset to the release
gh release upload v1 data.tar.gz
```

Get bigger datasets from assets attached to GitHub releases.

```bash
# If you work from outside the repo that has the assets, use the flag `--repo`
cd ~
gh release download v1 -p data.tar.gz --repo 2degreesinvesting/ds.cloud
```

#### Move data between a droplet and your computer with `scp`

(Consider using GitHub instead.)

Copy file from a remote host to local host:

```bash
$ scp root@from_host:file.txt /local/directory/
```

Copy file from local host to a remote host:

```bash
$ scp file.txt root@to_host:/remote/directory/
```

Copy directory from a remote host to local host:

```bash
$ scp -r root@from_host:/remote/directory/  /local/directory/
```

Copy directory from local host to a remote host:

```bash
$ scp -r /local/directory/ root@to_host:/remote/directory/
```

#### Move data between a droplet and a container

* Docker volumes: Bind mount.

#### Move data between multiple containers

* Docker volumes: Bind mount.
* Docker volumes: Managed volumes.


