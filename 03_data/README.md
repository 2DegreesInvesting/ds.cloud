### Increase disk space to store more data

> As an ananlyst I would like to increase my disk space as my data grows.

Resize: 

* Note resize options under "Storage-Optimized".

* Note resize options under "Disk, CPU and RAM": "This will increase the disk
size, CPU and RAM of your Droplet. This is a permanent change and cannot be
reversed."

* Create and attach a volume:

Volumes:

* Inspect droplet specifications and size: `df -h`

* Add a volume "demo_volume" to a droplet. [Volumes cost $0.10 GiB per month and
range from 1 GiB to 16 TiB (16,384 GiB). Charges accrue hourly for as long as
the volume
exists](https://docs.digitalocean.com/products/volumes/details/pricing/).

* Inspect droplet specifications and size: `df -h`

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

* The cloud manager runs something like this:

```bash
# Linda's container
docker run -d \
  --name linda -p 8787:8787 -e PASSWORD=123 \
  -v /mnt:/mnt  \
  -e ROOT=true \
  rocker/verse

# Mauro's container (note different port)
docker run -d \
  --name mauro -p 8788:8787 -e PASSWORD=321 \
  -v /mnt:/mnt  \
  -e ROOT=true \
  rocker/verse

# Show running containers
docker ps

# Inspect permission
ll /mnt/demo_volume
# Share
chmod a+rwx /mnt/demo_volume
ll /mnt/demo_volume
```

* The analyst login at `http://ipv4:port`  as "rstudio" with their password,
then share data with pins.

```r
# Linda writes some data

# install.packages("pins")
library(pins)

board <- board_folder("/mnt/demo_volume")
board |> pin_write(mtcars, "mtcars")
board |> pin_list()
```

```r
# Mauro uses the data that Linda created

# install.packages("pins")
library(pins)

board <- board_folder("/mnt/demo_volume")
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
