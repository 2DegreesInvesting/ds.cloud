### Move data around

Setup an example droplet at `174.138.4.109` (your ipv4 will be different).

```bash
# Connect to the droplet
ssh root@174.138.4.109

# Run a container from https://www.rocker-project.org/
docker run --rm -d --name gh \
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

Share bigger datasets as assets attached to GitHub releases:

> If you need to distribute large files within your repository, you can create releases on GitHub.com.  
> We don't limit the total size of the binary files in the release or the bandwidth used to deliver them. However, each individual file must be smaller than 2 GB.  
-- [Distributing large binaries](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github#distributing-large-binaries)


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

# Extract (x) the archive file (f) data.tar.gz
tar xf
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


