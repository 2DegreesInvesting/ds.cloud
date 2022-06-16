### Increase disk space to store more data

> As an ananlyst I would like to increase my disk space as my data grows.

* Inspect droplet specifications and size: `df -h`
* Add a volume to a droplet ($0.10 GiB/month).
* Find the volume.
* Inspect droplet specifications and size: `df -h`
* Resize volume.

Resources:

* <https://docs.digitalocean.com/products/volumes/quickstart/>

### Share data between team members with pins

```r
library(pins)

board_team <- function() {
  board_folder("/mnt/volume_tilt/pins")
}

board_team()
board_team() %>% pin_write(mtcars, "mtcars")
board_team() %>% pin_list()
board_team() %>% pin_read("mtcars")
```

### Share volume across droplests

https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-18-04

### Move data between a droplet and GitHub

### Move data between a container and GitHub

### Move data between a droplet and a container

### Move data between containers

### Move data between a droplet and your computer
