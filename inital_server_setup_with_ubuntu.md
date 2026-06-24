
# Initial Server Setup with Ubuntu

## .ssh/config

```
Host vmiss-hk
HostName server_ip
User bird
IdentityFile ~/.ssh/id_vmiss_hk
ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -S proxy_id %h %p
```

```shell
ssh vmiss-hk
```

Disabling Password Authentication

Once SSH key authentication is working, disable password authentication to prevent brute-force attacks.

`/etc/ssh/sshd_config`

```
PasswordAuthentication no
```

## Create a non-root user

When you first create a new Ubuntu server, you should immediately **create a non-root user with sudo privileges**, **configure SSH key authentication**, and **enable a firewall**. These three steps form the foundation of server security and prevent common attack vectors like brute-force password attempts and unauthorized access.


```shell
adduser bird
id bird

# Granting Administrative Privileges
# To add these privileges to our new user, we need to add the user to the sudo group.
# -a (append): Adds the user to the group without removing them from other groups
# -G (groups): Specifies the group(s) to add
usermod -aG sudo bird

# Enabling External Access for Your Regular User
# rsync: This will copy the root user’s .ssh directory, preserve the permissions, and modify the file owners. 
# Be sure that the source directory (~/.ssh) does not include a trailing slash
# --archive: preserves permissions, timestamps, and other attributes
# --chown: changes ownership to the new user
rsync --archive --chown=bird:bird ~/.ssh /home/bird
ls -la /home/bird/.ssh/
ssh bird@server_ip
```

## Disable Root Login 

```shell
sudo nano /etc/ssh/sshd_config
```

```
PermitRootLogin no
```

```shell
sudo systemctl restart ssh
```

## Setting Up a Basic Firewall


```shell
ufw app list
# Available applications:
#   OpenSSH

ufw allow OpenSSH # ufw allow ssh or ufw allow 22/tcp

ufw enable

ufw status
# Status: active

# To                         Action      From
# --                         ------      ----
# OpenSSH                    ALLOW       Anywhere
# OpenSSH (v6)               ALLOW       Anywhere (v6)

# If you plan to run a web server, you’ll need to allow HTTP and HTTPS traffic:
ufw allow 80/tcp # ufw allow 'Nginx Full'
ufw allow 443/tcp # ufw allow 'Apache Full'
```

> [Initial Server Setup with Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04)
>
> [How To Disable Root Login on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-disable-root-login-on-ubuntu-20-04)