# Mounting Volumes


## Adding a new physical drive to the system

1. Check if `drive` is present in system.
Run this to view all drive names:
``` 
sudo fdisk -l  
```
`-l` to list all partitions of all drives.

2. Check `partitions` on `disk`:
```
sudo gdisk <drive identifier>
or
lsblk
```
``` 
example: sudo gdisk /dev/sde
```

2. [Optional step] Delete `partitions`:
```
sudo gdisk <drive identifier> 
 type d
 type partition number
 type w  -> to delete pertition -> yes
```
3. Create new `partition`:
```
sudo gdisk <drive identifier> 
type n
type 1
confirm
type w to confirm partition
```

4. To see the `partition` name:
```
sudo fdisk -l
```

5. Get `drive` identifier:
```
sudo ls -la /dev/disk/by-uuid/
```
Or
```
blkid <drive identifier>
```

6. Format the `partition` with a `filesystem` type `ext4`:
```
sudo mkfs.ext4 <drive identifier> 
```
8. Create folder for drive in `/mnt`. 
All mounted drives shoult be kept as a folder in `/mnt` folder.
```
sudo mkdir /mnt/<myfolder>
```
7. `Auto-mount` the `drives` at `boot`.

Open `fstab`:
```
sudo nano /etc/fstab
```
Add this line:
```
UUID=<drive unique identifier>  /mnt/<myfolder> ext4 defaults 0 0
```



