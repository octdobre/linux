# :tv: Home Media Server with :vhs: PLEX:tm: Media Server :movie_camera:

If you are an avid movie-watcher and wish to have full control of your own media,
**PLEX Media Server** is the perfect tool for you. 

In this tutorial I will show you how to:
* :metal: Install PLEX Media Server
* :point_right:Setup media libraries correctly
* :clap: Install Plugins
* :pray: Troubleshoot problems



## :cd:Installation

Add **PLEX** `repository` and `key`:key: to the system:
```
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
```

`Upgrade` system and install **PLEX**:
```
sudo apt update ; sudo apt upgrade
sudo apt install plexmediaserver
```

Check the `status` of **PLEX** server :white_check_mark::
```
sudo systemctl status plexmediaserver
```

Create **PLEX** `firewall`:fire: profile:
```
sudo nano /etc/ufw/applications.d/plexmediaserver
```

Copy this to it:
```
[plexmediaserver]
title=Plex Media Server (Standard)
description=The Plex Media Server
ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp

[plexmediaserver-dlna]
title=Plex Media Server (DLNA)
description=The Plex Media Server (additional DLNA capability only)
ports=1900/udp|32469/tcp

[plexmediaserver-all]
title=Plex Media Server (Standard + DLNA)
description=The Plex Media Server (with additional DLNA capability)
ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp|1900/udp|32469/tcp
```

Add `profile` to `firewall`:fire::
```
sudo ufw app update plexmediaserver
```

Apply the new `firewall`:fire: `rules`:
```
sudo ufw allow plexmediaserver-all
```

Check `firewall`:fire: status:
```
sudo ufw status verbose
```

:heavy_check_mark:That's it! **PLEX:tm:** Media Server is now running on your `linux` machine:tada:. 
Now you need to do a first-time setup.

## :file_folder:Creating Media Folders

Create media `folders`:
```
sudo mkdir -p /opt/plexmedia/{movies,series}  
```

Set the `plex` user as the owner:key: of media `folders`:
```
sudo chown -R plex: /opt/plexmedia
```
If you have a `CIFS`:cd: mount in `FSTAB` set the `UID` and `GID` to `998`. This should be the `UID` and `GID` for the `plex` user and group. This could be different if another user with that `UID/GID` already existed so check in `passwd`.
```
sudo nano /etc/passwd
```


## :dvd:Setup the Server

Navigate here to start setting up your server: 

`http://YOUR_SERVER_IP:32400/web`

## :clipboard:Management

### :date:Updating
```
sudo apt update
sudo apt install --only-upgrade plexmediaserver
```

During the installation process, the official `Plex` `repository` may be disabled. 
To enable the `repository`, open the `plexmediaserver.list` file:
```
sudo nano /etc/apt/sources.list.d/plexmediaserver.list
```
Uncomment the line starting with “deb”:
```
# When enabling this repo please remember to add the PlexPublic.Key into the apt setup.
# wget -q https://downloads.plex.tv/plex-keys/PlexSign.key -O - | sudo apt-key add -
deb https://downloads.plex.tv/repo/deb/ public main
```

### :electric_plug:Plugins

Plugins are located in the folder:
```
"/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Plug-ins"
```

:point_right: Install :link:[**WebTools**](https://github.com/ukdtom/WebTools.bundle)

After installing **WebTools** navigate to:
`192.168.178.26:33400`

Remember to allow this port in the `UWF` `firewall`:fire: too:
```
sudo uwf allow 33400
```
You can use **WebTools** to install other :electric_plug:plugins.

### :family:Local Network Users

For local users you can create so-called `Managed users`:family:.
When creating such a user, you are still logging in to that computer 
with your admin credentials:key:.

You can set a `pin`:lock: for yourself and for `Managed Users`:family: 
to not login on your admin account and modify important **PLEX** `server settings`.

## :wrench:Troubleshooting

### :lock:Not Authorized Problem

If you just installed the **PLEX** Server but you see the page `NOT AUTHORIZED` when navigating to your **PLEX** Server then follow these steps:


:sailboat: Navigate to: 
```
cd "/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/"
```

Open the `Preferences.xml` file:
```
sudo nano Preferences.xml 
```
Add these `3` `attributes`: 
```
PlexOnlineMail="<your admin account email>" 
PlexOnlineToken="token"   
PlexOnlineUsername="<admin username >" 
```

To get the token login to `www.plex.tv` and navigate to `https://www.plex.tv/claim/`.
