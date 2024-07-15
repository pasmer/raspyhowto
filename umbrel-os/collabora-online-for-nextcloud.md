# Collabora online for Nextcloud

## Create a project folder

1. Connect to Umbrel OS using SSH. With root ($ sudo su) go to /home/umbrel/umbrel
2. Create a folder called "collabora".
3. Create a folder called "datacool" where create a file called "coolwsd.xml" as a copy from the original container (this step is optional)
4. Create a file "compose.yml" in /home/umbrel/umbrel/collabora such as:

```
version: "3"
services:
    code:
        tty: true
        ports:
            - 9980:9980
        environment:
            - domain=cloud\\.server\\.com
            - extra_params=--o:ssl.enable=true
        restart: always
        cap_add:
            - SYS_ADMIN
        volumes:
            - ./datacool/coolwsd.xml:/etc/coolwsd/coolwsd.xml
        image: collabora/code
```

from the folder /home/umbrel/umbrel/collabora run this:

```
docker compose up -d
```

Assumption: my nextcloud domain is: "cloud.server.com".

Finally, check the reverse proxy configuration for collabora
