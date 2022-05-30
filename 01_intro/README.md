# Introduction to cloud computing 

### What is cloud computing?

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

### Setup a droplet on DigitalOcean, with Docker on Ubuntu.

[Take an interactive tour of DigitalOcean](https://www.digitalocean.com/try/developer-brand#tour).

### Explore your droplet from the console

Orient yourself.

```bash
pwd
whoami
echo $HOME
cd ~
ls -A
ls /
docker images
```

Run RStudio

```bash
# https://www.rocker-project.org/
docker run -e PASSWORD=yourpassword --rm -p 8787:8787 rocker/rstudio
```

Point your browser to ipv4:8787. Log in with user/password rstudio/yourpassword. 

See also https://github.com/2DegreesInvesting/ds.docker
