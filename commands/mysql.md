# MySql

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
