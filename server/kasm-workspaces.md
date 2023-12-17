# Kasm Workspaces

## Install

Use Docker:

```
docker pull linuxserver/kasm
```

Run the docker (I changed the ports because they were already in use).

```
docker run -d --name=kasm --privileged -e KASM_PORT=843 -p 5000:3000 -p 843:443 -v datakasm:/opt -v profileskasm:/profiles -v inputkasm:/dev/input -v udevdatakasm:/run/udev/data --restart unless-stopped linuxserver/kasm
```

Go to: https://\<internal IP>:5000 and follow the installation process. After the installation the port 5000 will not be used anymore.

Configure a DNS entry to point to the right server.

Configure Nginx proxy like that:

```
server {  
              listen 443;
              server_name hub.merella.it;

		ssl_certificate		 /etc/letsencrypt/live/hub.merella.it/fullchain.pem;
		ssl_certificate_key	 /etc/letsencrypt/live/hub.merella.it/privkey.pem;

              location / {  
                proxy_cache_bypass $http_upgrade;
		# New
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_pass https://192.168.1.112:843;
		proxy_read_timeout 90;
  
               }  
}
```

Try to run Kasm: https://server





### Links:

{% embed url="https://hub.docker.com/r/linuxserver/kasm" %}

