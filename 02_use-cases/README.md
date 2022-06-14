# Use cases

### Use case: Scale up/down to meet computing demand

> As an analyst I would like to have a powerful computer, as cheaply as possible, and whenever I need it.

Resources:

* <https://docs.digitalocean.com/products/droplets/how-to/resize/>

### Use case: Quickly access a complete and isolated R environment

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

Log in as "rstudio" with "yourpassword" at `https://{ipv4}:8787`, e.g.
<http://174.138.4.109:8787/>.

```r
library(tidyverse)
library(rmarkdown)
```
  
A more complete computer does work out of the box.

```bash
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/verse
```

```r
library(tidyverse)
library(rmarkdown)
```

Resources:

* Rocker images: https://www.rocker-project.org/images/

### Use case: Run long processes unattended

> As an analyst I would like to have a way to run a long analysis unattended, so
that I can turn off my laptop and do something else while the process runs.

```bash
docker run -d --name myjob -e PASSWORD=123 -p 8787:8787 rocker/verse
```

* Create an RStudio project with a file in R/jobs.R

```r
usethis::create_project("abc")
```

```r
usethis::use_r("job.R")

# R/jobs.R
# Save one every 5 seconds.
x <- 1:100
for (i in seq_along(x)) {
  text <- as.character(i)
  file <- paste0(i, ".txt")
  writeLines(text, file)

  Sys.sleep(5)
}
```

* Source as Local Job ...

<img src=https://i.imgur.com/rWhFj49.png width=700/>

Note that _local jobs ... will be shut down if R is_ but _while your R session
is running jobs ... your R session will not be suspended (on RStudio Server)_.
-- <https://www.rstudio.com/blog/rstudio-1-2-jobs/>

* Monitor: How many files have been saved?

```bash
docker exec myjob ls /home/rstudio/abc -t
```

Resources:

* <https://solutions.rstudio.com/r/jobs/> 
* <https://www.rstudio.com/blog/rstudio-1-2-jobs/>
* <https://callr.r-lib.org/>

### Use case: Host shiny apps

* Create apps in an environment like this one:

```bash
# https://www.rocker-project.org
docker run --rm -d -p 8787:8787 \
    -v /mnt:/mnt \
    --name rstudio \
    -e ROOT=true \
    -e PASSWORD=123 \
    rocker/verse
```

* Move the apps to /srv/shinyapps/

```bash
# From container
cd ~
sudo mv app /mnt

# From host (droplet)
mkdir /srv/shinyapps
mv /mnt/app /srv/shinyapps
```

```bash
# https://hub.docker.com/r/rocker/shiny
docker run --rm -d -p 3838:3838 \
    --name shiny \
    -v /srv/shinyapps/:/srv/shiny-server/ \
    -v /srv/shinylogs/:/var/log/shiny-server/ \
    rocker/shiny
```

Access the apps at `http://{ipv4}:3838/{app-dir}`, e.g.:

* <http://174.138.4.109:3838/app1/>
* <http://174.138.4.109:3838/app2/>

Resources:

* <https://www.rocker-project.org/images/>
* <https://www.rocker-project.org/use/shared_volumes>.
* <https://hub.docker.com/r/rocker/shiny>
* <https://github.com/2DegreesInvesting/ds.docker>.

