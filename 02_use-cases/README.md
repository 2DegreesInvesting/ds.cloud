# Use cases

### Use case: Scale up/down to meet computing demand

> As an analyst I would like to have a powerful computer, as cheaply as possible, and whenever I need it.

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

Log in as "rstudio" with "yourpassword" at `https://{ipv4}:8787`, e.g. <https://174.138.4.109:8787>.

```r
> library(tidyverse)
Error in library(tidyverse) : there is no package called ‘tidyverse’
> library(rmarkdown)
Error in library(rmarkdown) : there is no package called ‘rmarkdown’
> 
```
  
A more complete computer does work out of the box.

```bash
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/verse
```

```r
library(tidyverse)
library(rmarkdown)
```

### Use case: Run long processes unattended

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

Note:

> Local jobs run as non-interactive child R processes of your main R process,
which means that they will be shut down if R is. 

But:

> While your R session is running jobs ... Your R session will not be suspended
(on RStudio Server)

To be sure maybe best to run your script with R outside of RStudio?

```bash
# Test
docker exec myjob Rscript -e "print('hello world')"

# Run
docker exec myjob Rscript -e "source('/home/rstudio/abc/R/job.R')"
```

Monitor progress.

```bash
# From outside the container
# What docker containers are running?
docker ps

# From inside the container
# What processes are running inside the container myjob?
docker exec myjob ps -F
# How many files have been saved?
docker exec myjob ls -t /home/rstudio/abc | head
```

Learn more:

* <https://solutions.rstudio.com/r/jobs/> 
* <https://www.rstudio.com/blog/rstudio-1-2-jobs/>
* <https://callr.r-lib.org/>
