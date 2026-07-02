# [mihomo](https://github.com/MetaCubeX/mihomo)

## Install

```bash
wget https://github.com/MetaCubeX/mihomo/releases/download/v1.19.27/mihomo-linux-amd64-compatible-v1.19.27.gz

# -k: keep the original file
gunzip -k mihomo-linux-amd64-compatible-v1.19.27.gz

sudo mv mihomo-linux-amd64-compatible-v1.19.27.gz /usr/local/bin/mihomo

# chomd +x:
sudo chomd +x /usr/local/bin/mihomo

sudo mkdir -p /etc/mihomo

# quick configuration
# -L: follow redirects
# -o: write output to a file
curl -L "https://wiki.metacubex.one/example/mrs" -o /etc/mihomo/config.yaml
```

## Using systemd

`/etc/systemd/system/mihomo.service`

```ini
[Unit]
Description=mihomo Daemon, Another Clash Kernel.
After=network.target NetworkManager.service systemd-networkd.service iwd.service

[Service]
Type=simple
LimitNPROC=500
LimitNOFILE=1000000
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_RAW CAP_NET_BIND_SERVICE CAP_SYS_TIME CAP_SYS_PTRACE CAP_DAC_READ_SEARCH CAP_DAC_OVERRIDE
Restart=always
ExecStartPre=/usr/bin/sleep 1s
ExecStart=/usr/local/bin/mihomo -d /etc/mihomo
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

```bash
# reload systemd
systemctl daemon-reload
systemctl enable --now mihomo
systemctl reload mihomo

# -e: jump to the end of the log in the pager
# -f: follow the log in real time
journalctl -u mihomo -o cat -e
journalctl -u mihomo -o cat -f
```

## Web dashboard

http://127.0.0.1:9090
