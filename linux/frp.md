# Frp

## Install

```bash
wget https://github.com/fatedier/frp/releases/download/v0.69.1/frp_0.69.1_linux_amd64.tar.gz

# -x extract files from archive.
# -z decompress via gzip (`.gz`).
# -f specify the archive filename.
tar -xzf frp_0.69.1_linux_amd64.tar.gz
```

## Configuration

### frps

```bash
sudo cp frps /usr/local/bin
sudo mkdir /etc/frp
sudo cp frps.toml /etc/frp
```

`frps.toml`

```toml
bindPort = 7000
auth.token = "12345"
```

`/etc/systemd/system/frps.service`

```ini
[Unit]
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /usr/bin/frps -c /etc/frp/frps.toml

[Install]
WantedBy = multi-user.target
```

ufw

```bash
sudo ufw allow 7000/tcp
sudo ufw allow 6000/tcp
```

### frpc

```bash
sudo cp frpc /usr/local/bin
sudo mkdir /etc/frp
sudo cp frpc.toml /etc/frp
```

`frpc.toml`

```toml
serverAddr = frps_addr
serverPort = 7000
auth.token = "12345"
transport.proxyURL = "http://127.0.0.1:7890"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000
```

`/etc/systemd/system/frpc.service`

```ini
[Unit]
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /usr/bin/frpc -c /etc/frp/frpc.toml

[Install]
WantedBy = multi-user.target
```

---

```bash
sudo systemctl start frps # or frpc
sudo systemctl stop frps
sudo systemctl restart frps
sudo systemctl status frps

# Start automatically on boot
sudo systemctl enable frps
sudo systemctl enable --now frps
```

`wget` is a command-line download utility. It fetches the file from the given URL and saves it to the current directory.

`tar` manages archive files.

`/bin`: for essential user command binaries used by all users, such as cp, ls, sh, mount.

`/etc`: for host-specific system configuration files and must not contain binary executables.
