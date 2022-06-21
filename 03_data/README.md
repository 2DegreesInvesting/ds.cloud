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

* Find the volume. ["Volumes are auto-mounted into the /mnt
directory"](https://docs.digitalocean.com/products/volumes/how-to/create/).

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
mkdir /mnt/volume/shared/pins

# Share it with everyone: Add read/write/execute permission for all
chmod a+rwx /mnt/volume/shared/pins
ll /mnt/volume
```

* The cloud manager creates a server for each team member:

```bash
# Linda's container on port 8787
docker run -d \
  --name linda -p 8787:8787 -e PASSWORD=123 \
  -v /mnt:/mnt  \
  -e ROOT=true \
  rocker/verse

# Mauro's container on port 8788
docker run -d \
  --name mauro -p 8788:8787 -e PASSWORD=321 \
  -v /mnt:/mnt  \
  -e ROOT=true \
  rocker/verse

# Show running containers
docker ps
```

* The analysts login at `http://ipv4:their.port`  with username "rstudio" and
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

### Moving data around

* Move data between a droplet and a container
* Move data between containers
* Move data between a droplet or container and GitHub
* Move data between a droplet and your computer
