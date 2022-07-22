# Installing and Managing Docker Engine

Docker engine is a service to manage containers, volumes, and private virtual networks.

## Installing

Upgrade and allow HTTPS repositories.

```
sudo apt-get update
```
```
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
Download Docker gpg key:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Set Stable Repository:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null	
```
Update again:
```
sudo apt-get update
```
Install Docker:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

The service is not yet prepared.

## Uninstalling

Before uninstalling Docker, make sure that all images/containers/volumes are deleted:exclamation:

```
sudo apt-get purge docker-ce docker-ce-cli containerd.io
```

Remove leftovers:
```
sudo rm -rf /var/lib/docker
```
```
sudo rm -rf /var/lib/containerd
```

## Post Install docker cli permission
--------------------------------------------------------------------------------
Create a user group for docker.
```
sudo groupadd docker
```
Give permissions to the user to run docker commands without sudo.
```
sudo usermod -aG docker 
```

## Configure docker to start on boot (by default docker engine starts automatically)
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

To Disable:
```
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
## View details about Docker
```
docker info
```

## Test the Docker installation is correct

Download this image 
```
docker pull yeasy/simple-web
```
```
docker run -d -p HOSTPORTHERE:80 yeasy/simple-web:latest
```

Use the browser to navigate to 127.0.0.1:HOSTPORTHERE

## Enable IPv6 support

Create or open this file
```
/etc/docker/daemon.json
```

Paste the contents/ put your own ipv6 subnet
```
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8:1::/64"
}
```

Reload the docker service:
```
sudo systemctl reload docker
```
Sometimes you might need to reboot here.
