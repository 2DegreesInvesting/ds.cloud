# Introduction to cloud computing 

### What is cloud computing?

Cloud computing is the delivery of computing resources as a service,
meaning that the resources are owned and managed by the cloud provider
rather than the end user. 

The computing resources might be:

* browser-based software applications, e.g. a Shiny app;
* data storage, e.g. Dropbox;
* servers to analyze data, e.g. a DigitalOcean droplet running RStudio
Server.

### What are the benefits?

As an organization:

* Save $. We can buy cheap, basic computers, and rent powerful servers when we need them.
* We don't worry about your computers getting old, breaking, or loosing value.

As researchers:

* We can access the computing resources we need when you need them.
* We can avoid the slow, painful HR processes to required to buy the
hardware or install the sofware we need.

As developers:

* We can easily get the right tool for the job at hand. 
* We don't waste time building low-level infrastructure. Instead we
build on top of robust "templates".

As students/interns.

* We can access easily a functional computing environment and data.
* Don't waste our limited time installing software or downloading data.

### Setup DigitalOcean droplet with RStudio

[Take an interactive tour of DigitalOcean](https://www.digitalocean.com/try/developer-brand#tour).

* Sign up/login to https://www.digitalocean.com/
* Create/choose a project.
* Create > Droplets > Marketplace > Docker > ... (defaults)
 
### Explore your droplet from the console

Note this message:

```bash
Welcome to DigitalOcean's 1-Click Docker Droplet.

...

* You can SSH to this Droplet in a terminal as root: ssh root@the.server.ipv4.address

...

```

Your droplet is like any other system running Ubuntu.

```bash
whoami
cd ~
ls -A
ls /
```

(https://github.com/2DegreesInvesting/ds.terminal)

And it has Docker installed.

```bash
docker run hello-world
docker images
```

You can use it to run any docker image, e.g. rocker/rstudio

```bash
# https://www.rocker-project.org/
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/rstudio
```

Point your browser to [ipv4]:8787. Log in with user/password rstudio/yourpassword. 

(https://github.com/2DegreesInvesting/ds.docker)
