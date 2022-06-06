# Use cases

### Scale up, scale down, and turn off

> As an analyst I would like to have a powerful computer, as cheaply as possible, and whenever I need it.

### Run RStudio with useful packages pre-installed

> As an analyst in a new project I would like to have all the software I need
without messing up with the software I use in other analyses.

Login to your droplet

```bash
# ssh root@174.138.4.109 
ssh root@your.ipv4.address
```

Use a docker image that gives you most of what you need ([see great options](https://www.rocker-project.org/images/)).

Say you want to use the tidyverse and rmarkdown. A basic computer doesn't work
out of the box.

```bash
# https://www.rocker-project.org/
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/rstudio
```

Go to `https://{ipv4}:8787` and log in as "rstudio" with "yourpassword".

```R
> library(tidyverse)
Error in library(tidyverse) : there is no package called ‘tidyverse’
> library(rmarkdown)
Error in library(rmarkdown) : there is no package called ‘rmarkdown’
> 
```

A more complete computer does work out of the box.

```r
library(tidyverse)
library(rmarkdown)
```

### How to move data to/from RStudio on the cloud?

* Upload a file.

* Upload a directory.

```bash
# Make an example directory
mkdir -p dir && touch dir/a dir/b

# Convert a directory into a file
tar -cf dir.tar dir
```

* Clone public/private repositories from GitHub.

```bash
git clone git@github.com:2DegreesInvesting/ds.cloud.git
```

* Clone public/private repositories from GitHub:

    * Setup keys for SSH ([how](https://happygitwithr.com/ssh-keys.html))
    * Setup a github pat: `usethis::gh_token_help()` and follow instructions.

### How to move data to/from the server?

```bash
# From the host
touch ~/abc.txt

# Replace with your ipv4 address
# From host to server
scp ~/abc.txt root@174.138.4.109:~
# From server to host
scp root@174.138.4.109:~/abc.txt ~
```

<https://haydenjames.io/linux-securely-copy-files-using-scp/>

### How to reuse data across containerized porjects?

* `-v /mnt:/mnt.

```bash
# See also https://docs.docker.com/engine/reference/run/
docker run -d --rm -v /mnt/mauro:/mnt/mauro -e ROOT=true -e PASSWORD=123 -p 8787:8787 rocker/verse
```

From the container.

```bash
touch /mnt/from-container
```

From the host of the container -- i.e. the server (droplet) on the cloud.

```bash
ls /mnt
touch /mnt/from-server
```

From the container.

```bash
ls /mnt
```

```bash
docker run --rm -ti -e PASSWORD=123 -v /mnt:/mnt rocker/verse bash
```


###


* Use case: Run long processes unattended.
* Use case: Publish/access private data.
* Use case: Publish/access static websites -- public and private.
* Use case: Publish/access shiny apps

