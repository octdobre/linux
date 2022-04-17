# Install OpenSpeedTest Server on Docker

Pull the latest image:
```
docker pull openspeedtest/latest 
```
Create container:
```
docker run --restart=unless-stopped --name openspeedtest -d -p 3000:3000 -p 3001:3001 --network v-six openspeedtest/latest 
```

Create UFW profile
```
sudo nano /etc/ufw/applications.d/openspeedtestserver
```

Paste in:
```
[openspeedtestserver]
title=Open SpeedTest Server (Standard)
description=The Open SpeedTest Server
ports=3000/tcp|3001/tcp
```

Allow in UFW:
```
sudo ufw app update openspeedtestserver
```
```
sudo ufw allow openspeedtestserver
```