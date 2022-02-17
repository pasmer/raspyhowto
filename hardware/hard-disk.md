# Hard Disk

### Support for NTFS

To add support for the NTFS file system to our Raspberry Pi all we need to do is enter the following command into the terminal to install the [NTFS-3G package](https://packages.debian.org/search?keywords=ntfs-3g).

```
sudo apt-get install ntfs-3g
```

### Check HD

Check all infos about the HD:

```
sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL
```

### FSTAB file

To mount an external HD every reboot, insert the following line in `/etc/fstab` file:

```
UUID=800A5A900A5A82D8 /media/Repo ntfs-3g nofail,uid=1000,gid=1000,umask=0022,noatime 0 0
```

### How to Repair

```
sudo -i # to get Root
fdisk -l

# Other way:
fsck -y /dev/sdb1
```
