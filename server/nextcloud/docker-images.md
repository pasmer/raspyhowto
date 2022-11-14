# Docker images

Website NextcloudPi

{% embed url="https://nextcloudpi.com/" %}

### Run the docker:

```
sudo docker run -d -p 4443:4443 -p 643:443 -p 8080:80 -v /media/cloud/datacloud:/data/nextcloud/data -v ncdata:/data --name nextcloudpi ownyourbits/nextcloudpi cloud.merella.it
```

### Create MySql database

```
sudo mysql -u root -p
```

On MySql consolle:

```
CREATE DATABASE nextclouddck;

GRANT ALL PRIVILEGES ON nextclouddck.* TO 'nextcloudusr'@'localhost' IDENTIFIED BY 'PASSWORD';

FLUSH PRIVILEGES;

```

