# Networking :computer::electric_plug::computer:

This section is dedicated to managing the network resources in linux
and describing tools to debug networking issues.

## UFW 
```
sudo apt install ufw
```

UFW or "Uncomplicated Firewall":fire: is a service to manage network filtering.

When using linux:penguin: to host services, it is important to have control over network connections:electric_plug:. 

Its main purpose is to allow:white_check_mark: and deny ports.

With UFW you can do that manually with commands or create application profiles.


### Manually managing UFW :fire:
View all rules:memo::
```
sudo ufw status verbose
```

You can ALLOW:white_check_mark:/DENY:x: ports.
```
sudo ufw allow 22
```
By default, the protocol used will be TCP:large_blue_diamond:.

Allow with specific protocol:
```
sudo ufw allow 22/tcp
sudo ufw allow 22/udp
```
Allow port range:
```
sudo ufw allow 20000:20010
```
Allow specific IP address:
```
sudo ufw allow from 192.168.0.20
```
Allow :point_right:specific IP to specific port:
```
ufw allow from 192.168.0.20 to any port 22
```
Allow subnet:
```
sudo ufw allow from 192.168.0.1/24
```
Deny :point_right:specific protocol:
```
sudo ufw deny http
```
View rules numbered :one::two::three::
```
sudo ufw status numbered
```
Using the deny command, does not delete the entry, it keeps the rule but blocks the connection
based on the rule. To remove the rule you must use the DELETE command.

Delete rules by number:
```
sudo ufw delete 2
```
Delete Rules by port
```
sudo ufw delete allow 22/tcp
```

By default, the rules are added for both IPv:four: and IPv:six:.

You must specify an address range to select a specific IP version.

IPv:four: only
```
ufw allow from 0.0.0.0/0 to any port 22 
```
IPv:six: only
```
ufw allow from ::/0 to any port 22 
```

### Creating application:pager: profiles

Some services need multiple ports to be configured and managed easily.

With UFW:fire: application profiles you can do just that.

UFW:fire: application profiles live in: 
```
/etc/ufw/applications.d
```
Create a profile:boy::
```
sudo nano /etc/ufw/applications.d/generic-service
```
Contents should look like this:
```
[generic-service]
title=Generic Service
description=The Generic Server
ports=20001/tcp
```

Add the profile to UFW:fire:(rules will not be applied yet:exclamation:):
```
sudo ufw app update generic-service
```
Activate the rule by name:
```
sudo ufw allow generic-service
```
Now check UFW for the rule:
```
sudo ufw status verbose
```

## Documentation :books:

:point_right: :link: [UFW](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)