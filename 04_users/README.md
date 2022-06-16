### How to Upload SSH Public Keys to a DigitalOcean Account

https://docs.digitalocean.com/products/droplets/how-to/add-ssh-keys/to-account/

### How to permit multiple users access to the same server/droplet?

* Add an ssh key to the DigitalOcean account: 
https://cloud.digitalocean.com/account/security

https://www.digitalocean.com/community/questions/how-to-permit-multiple-users-access-to-the-same-server-droplet:

* The user’s machine has the private portion of the ssh key

* The user’s machine has a terminal program that is configured to find the private key

* The server has an account for the user

* The user account on the server has a directory like ~username/.ssh with read/write/exec permissions for the user only (all other permissions are off)

* There is a file ~username/.ssh/authorized_keys with read/write for the user only

* One of the lines in the authorized_keys file is the public portion of the ssh key that is associated with the private portion held on the user’s machine

* The user’s terminal program on the user’s machine is configured to login using the server account name

### Enable docker without `sudo`

* Add user to docker group: https://docs.docker.com/engine/install/linux-postinstall/

```bash
sudo usermod -aG docker $USER
```
