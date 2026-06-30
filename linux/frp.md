## Install frp on ubuntu

`wget` is a command-line download utility. It fetches the file from the given URL and saves it to the current directory.

`tar` manages archive files.

```shell
wget https://github.com/fatedier/frp/releases/download/v0.69.1/frp_0.69.1_linux_amd64.tar.gz

# -x extract files from archive.
# -z decompress via gzip (`.gz`).
# -f specify the archive filename.
tar -xzf frp_0.69.1_linux_amd64.tar.gz

sudo cp frps /usr/bin/
sudo mkdir /etc/frp
sudo cp frp*.toml /etc/frp

sudo nano /etc/frp/frps.toml
# bindPort = 7000

sudo nano /etc/systemd/system/frps.service
# [Unit]
# Description = frp server
# After = network.target syslog.target
# Wants = network.target

# [Service]
# Type = simple
# ExecStart = /usr/bin/frps -c /etc/frp/frps.toml

# [Install]
# WantedBy = multi-user.target

sudo ufw allow 7000/tcp
sudo ufw allow 8080/tcp

sudo systemctl start frps
sudo systemctl stop frps
sudo systemctl restart frps
sudo systemctl status frps

# Start automatically on boot
sudo systemctl enable frps
sudo systemctl enable --now frps

# Listing Loaded Services
systemctl list-units --type=service
systemctl list-units --type=service --state=running
```

`/bin`: for essential user command binaries used by all users, such as cp, ls, sh, mount.

`/etc`: for host-specific system configuration files and must not contain binary executables.

> [Filesystem hierarchy standard](https://ubuntu.com/project/docs/how-ubuntu-is-made/concepts/filesystem-hierarchy-standard)

server B:

frpc.toml

```
serverAddr = server_ip
serverPort = 7000

[[proxies]]
name = "demo"
type = "tcp"
localIP = "127.0.0.1"
localPort = 4173
remotePort = 8080
```

```shell
./frpc -c ./frpc.toml
```

```shell
curl http://server_ip:8080
```
