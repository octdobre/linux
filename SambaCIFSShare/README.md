# Samba:cd: and CIFS:dvd: Shares

Shares are used to transfer `files` from one computer to another.

## Auto-mount Shares using `FSTAB`

Using `FSTAB` we can set up the `operating system` to `auto-mount` the `share` on system `boot`.

Note that if the share is not available on `boot`, the system might take longer to `boot`.

If the system has `booted` and the `share` was not available, it will not try to `re-mount` it.
You must manually `re-mount` it. For this feature check `AUTOFS`.


### How to setup `FSTAB` auto-mount:

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


# Auto-mount Shares using AUTOFS

* //TODO



:point_right::link:[How to create a Samba share on Windows](TODO)