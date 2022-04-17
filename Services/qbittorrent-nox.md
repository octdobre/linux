# Installing headless qbittorrent with Web UI

## Installing

Make sure to update all:
```
sudo apt update && sudo apt upgrade -y
```
Add Source repository:
```
sudo add-apt-repository ppa:qbittorrent-team/qbittorrent-stable -y && sudo apt update
```
Install qbittorrent-nox:
```
sudo apt install qbittorrent-nox
```

Create user and group:
```
sudo adduser --system --group qbittorrent-nox
```
```
sudo adduser <your-username> qbittorrent-nox
```

Create Service:
```
sudo nano /etc/systemd/system/qbittorrent-nox.service
```
Paste in:
```
[Unit]
Description=qBittorrent Command Line Client
After=network.target

[Service]
#Do not change to "simple"
Type=forking
User=qbittorrent-nox
Group=qbittorrent-nox
UMask=007
ExecStart=/usr/bin/qbittorrent-nox -d --webui-port=8080
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Create UFW rule:
```
sudo nano /etc/ufw/applications.d/qbittorrent-nox
```

Paste in:
```
[qbittorrent-nox]
title=Qbittorrent Nox Web
description=The Qbittorrent-Nox Torrent Client and Web Server
ports=25000/tcp
```
Allow in UFW:
```
sudo ufw app update qbittorrent-nox
```
```
sudo ufw allow qbittorrent-nox
```
Reload systemd daemon:
```
sudo systemctl daemon-reload
```
Enable the service:
```
sudo systemctl enable qbittorrent-nox
```
```
sudo systemctl start qbittorrent-nox
```
Check status:
```
systemctl status qbittorrent-nox
```

## Update
```
sudo apt update ; sudo apt upgrade
```

## Remove
Clear the repository:
```
sudo add-apt-repository --remove ppa:qbittorrent-team/qbittorrent-stable
```
```
sudo apt remove qbittorrent
```

## Troubleshooting

* By default, the torrents will be saved in the home directory of the qbittorrent user.
* When the torrents must be used by other services such as PLEX, Emby, Jellyfin, then 
don,t save the torrents in the home folder of the qbittorrent user. Instead, save them
in a folder where the qbittorrent user has `read write execute` permission.