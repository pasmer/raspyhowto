# Docker images

Website NextcloudPi

{% embed url="https://nextcloudpi.com/" %}

### Run the docker:

```
sudo docker run -d -p 4443:4443 -p 843:443 -p 8085:80 -v /media/cloud/NCProot/nextcloud/data:/opt/data -v ncpdata:/data --restart always --name nextcloudpi nextcloudpi64 cloud.merella.it
```

### Instruction to create the image



#### Data folder (USB mounted as www-data rights)

The HDD is mounted on /media/cloud

The HDD data folder is here: /media/cloud/NCProot/nextcloud/data

The Docker data folder is here: /opt/data





Create a soft link

```
sudo -u www-data ln -s /opt/data /var/www/nextcloud/data
```



