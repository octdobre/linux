# Install NetData monitoring Server on baremetal

Install package:
```
sudo apt install netdata -y
```
Get the ip of the network device using:
```
ip addr
```

Open netdata conf and add the ip of the network device:
```
sudo nano /etc/netdata/netdata.conf
```
Restart the service:
```
sudo systemtcl restart netdata
```
Create UFW profile
```
sudo nano /etc/ufw/applications.d/netdataserver
```
Paste in:
```
[netdataserver]
title=Netdata Monitoring Server (Standard)
description=The Netdata Monitoring Server
ports=19999/tcp
```

Allow in UFW:
```
sudo ufw app update netdataserver
```
```
sudo ufw allow netdataserver
```