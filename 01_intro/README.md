# Introduction to cloud computing 

### What is cloud computing?

Cloud computing is the delivery of computing resources as a service,
meaning that the resources are owned and managed by the cloud provider
rather than the end user. Those resources may be:

* browser-based software applications, e.g. Netflix;
* data storage for photos and other digital media, e.g. Dropbox;
* servers to run computations, for research, e.g. a DigitalOcean droplet
with RStudio.

### What are the benefits?

For an organization:

* Save $. Buy basic, cheap computers, and rent powerful ones when you
need them.
* Don't worry about your computers getting old, breaking, or loosing value.

For researchers:

* Access the computing resources you need when you need them.
* Avoid the slow, painful HR processes to buy/install the tools you need.

For developers:

* Access immediately the right tool for the job at hand. 
* Don't waste time building low-level infrastructure. Instead build on
top of robust "templates".

For students/interns.

* Get immedate access to a functional computing environment and data.
* Don't waste limited time installing software or downloading data.

### Setup DigitalOcean droplet with RStudio

[Take an interactive tour of DigitalOcean](https://www.digitalocean.com/try/developer-brand#tour).

* Sign up/login to https://www.digitalocean.com/
* Create/choose a project.
* Create > Droplets > Marketplace > Docker > Basic ($5) > Region > SSH >
Hostname > Create Droplet.
 
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
