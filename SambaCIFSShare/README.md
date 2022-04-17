# Samba:cd: and CIFS:dvd: Shares

Shares are used to transfer `files` from one computer to another.

# Table of Contents
* [Hosting](#hosting)
* [Mounting](#mounting)

## Hosting

### Hosting a Samba:cd: share

You can host a samba share on linux by installing the samba service and selecting which 
folders:file_folder: to publish with other computers:computer:.
```
sudo apt install samba
```
:point_right:From this point on you can choose to write the entire configuration in:
```
/etc/samba/smb.conf
```
Or you could create a backup:floppy_disk: and write a clean file from scratch.
```
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
```
After installing, stop:x: the service for us to modify the configuration file.
```
sudo systemctl stop smbd
``` 
Open the configuration file:memo::
```
sudo nano /etc/samba/smb.conf
```
Add the following contents:memo::
```
# Settings for Samba service
[global]
server string = <name here> File Server
workgroup = WORKGROUP
security = user
#map to gues = Bad User
name resolve order = bcast host
# External file to define shares
include = /etc/samba/shares.conf  
```
Create the external shares file.
```
sudo nano /etc/samba/shares.conf 
```
And add your share:
```
[LocalMovies]
path = <path to folder you want to share>
force user = smbuser
force group = smbgroup
create mask = 0777
create mode = 0777
directory mask = 0777
force directory mode = 0777
public = yes
writeable = yes
```

Create the group:family: and the user:raising_hand:. You can also use whatever group or user you wish.
```
sudo groupadd --system smbgroup   
# check group
cat /etc/group
```
```
sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser
#check user
cat /etc/passwd
```
Make sure the user and group have ownership and permissions:key:.
```
sudo chown -R smbuser:smbgroup <folder>
```

Start the service:white_check_mark:
```
sudo systemctl start smbd
sudo systemctl status smbd
```

Latelly, the samba service installer comes with a script to install a UFW profile.
This might not activate always so let's activate it.
```
sudo ufw app update samba
sudo ufw allow samba
```

Adding a user:raising_hand: to the samba share. This will enable other computers to login with this user to the share.
```
sudo smbpasswd -a <user>
```

## Mounting

On linux in order to view Samba shares from other pc's we have to mount them to a specific folder locally.

### Auto-mount Shares using `FSTAB`

Using `FSTAB` we can set up the `operating system` to `auto-mount` the `share` on system `boot`.

Note that if the share is not available on `boot`, the system might take longer to `boot`.

If the system has `booted` and the `share` was not available, it will not try to `re-mount` it.
You must manually `re-mount` it. For this feature check `AUTOFS`.


#### How to setup `FSTAB` auto-mount:

Install the `cifs_utils`.
```
sudo apt install cifs_utils
```

For security reasons do not store credentials in `/etc/fstab`.

Create a `credentials`:key: file:
```
sudo touch /home/myuser/.credentials 
```

Open the `file` and populate it with the `credentials`:key::
```
sudo nano /home/myuser/.credentials
```

Example:
```
username=name_of_smb_user
password=password
domain=WORKGROUP
```
By default domain:office: is `WORKGROUP` on **Windows** but check to be sure.

Open `FSTAB` and add this with your specific info:
```
Format
//hostname/sharedfolder /folderinlinuxyouwanttomount  cifs credentials=/location_of_creds_file,iocharset=utf8,dir_mode=0777,file_mode=0777,uid=998,gid=998 0 0
```

For `UID` and `GID` set the `UID/GID` of the `User` and `Group` you want to be the owner of
and `CIFS` will automatically make the `UID/GID` owner of this `folder` on `linux`.
  
Example:  
```
//MyDesktop/Movies /media/movies cifs credentials=/home/myuser/.credentials,iocharset=utf8,dir_mode=0777,file_mode=0777,uid=998,gid=998 0 0
```

From this point either restart Ubuntu or run this command `sudo mount -a `.


### Auto-mount Shares using AUTOFS

* //TODO



:point_right::link:[How to create a Samba share on Windows](https://www.youtube.com/watch?v=AxhSvBg0dTM)