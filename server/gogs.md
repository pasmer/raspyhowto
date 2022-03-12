---
description: Private Git Web Portal in Raspberry PI
---

# Gogs

## Installation

Install also Git, as required:

```
sudo apt install git
```

Download the zip installation file in `/opt`

Database required: MySQL, PostgreSQL, TiDB

### Database setup

From terminal:

`sudo mysql -uroot -p`

On MySql terminal:

```
create database gogsDb;
GRANT ALL PRIVILEGES ON gogs.* TO 'goguser'@'localhost' IDENTIFIED BY
'password';
flush privileges;
QUIT
```

### Service and folders

Use a new user for the server, called 'git'.

`sudo adduser --disabled-login git`

Then create folder with the right rights:

```
sudo chown -R git:git /opt/gogs
sudo chown -R git:git /opt/gogs-repositories
```

Create a new service under `/etc/systemd/system/` folder

```
sudo nano /etc/systemd/system/gogs.service
```

and add following lines:

```
[Unit]
Description=Gogs service script
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=git
ExecStart=/opt/gogs/gogs web

[Install]
WantedBy=multi-user.target

```

Close and save. Configure service to start on reboot:

```
sudo systemctl enable gogs.service
```

Start your new service with command:

```
sudo systemctl start gogs.service
```

In case of changes, please use:

`sudo systemctl daemon-reload`

``

