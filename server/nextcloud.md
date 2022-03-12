# Nextcloud

### Folder in cloud

Create a new folder (with Root permission) in `/media` then change the permissions:

```
chmod 770 /media/cloud
chown -R www-data:www-data /media/cloud
```

Notice: the folder /media/cloud is the mount folder included in [Fstab](../hardware/hard-disk.md#fstab-file) file.
