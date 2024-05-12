# Nginx Config Example 2

Add the below template to your nginx config.

map $http\_upgrade $connection\_upgrade { default upgrade; '' close; }

server { listen 80; listen \[::]:80; # comment to disable IPv6

```
if ($scheme = "http") {
    return 301 https://$host$request_uri;
}

listen 443 ssl http2;      # for nginx versions below v1.25.1
listen [::]:443 ssl http2; # for nginx versions below v1.25.1 - comment to disable IPv6

# listen 443 ssl;      # for nginx v1.25.1+
# listen [::]:443 ssl; # for nginx v1.25.1+ - keep comment to disable IPv6

# http2 on;                                 # uncomment to enable HTTP/2        - supported on nginx v1.25.1+
# http3 on;                                 # uncomment to enable HTTP/3 / QUIC - supported on nginx v1.25.0+
# quic_retry on;                            # uncomment to enable HTTP/3 / QUIC - supported on nginx v1.25.0+
# add_header Alt-Svc 'h3=":443"; ma=86400'; # uncomment to enable HTTP/3 / QUIC - supported on nginx v1.25.0+
# listen 443 quic reuseport;       # uncomment to enable HTTP/3 / QUIC - supported on nginx v1.25.0+ - please remove "reuseport" if there is already another quic listener on port 443 with enabled reuseport
# listen [::]:443 quic reuseport;  # uncomment to enable HTTP/3 / QUIC - supported on nginx v1.25.0+ - please remove "reuseport" if there is already another quic listener on port 443 with enabled reuseport - keep comment to disable IPv6

server_name <your-nc-domain>;

location / {
    proxy_pass http://127.0.0.1:11000$request_uri;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Port $server_port;
    proxy_set_header X-Forwarded-Scheme $scheme;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Accept-Encoding "";
    proxy_set_header Host $host;

    client_body_buffer_size 512k;
    proxy_read_timeout 86400s;
    client_max_body_size 0;

    # Websocket
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
}

# If running nginx on a subdomain (eg. nextcloud.example.com) of a domain that already has an wildcard ssl certificate from certbot on this machine, 
# the <your-nc-domain> in the below lines should be replaced with just the domain (eg. example.com), not the subdomain. 
# In this case the subdomain should already be secured without additional actions
ssl_certificate /etc/letsencrypt/live/<your-nc-domain>/fullchain.pem;   # managed by certbot on host machine
ssl_certificate_key /etc/letsencrypt/live/<your-nc-domain>/privkey.pem; # managed by certbot on host machine

ssl_session_timeout 1d;
ssl_session_cache shared:MozSSL:10m; # about 40000 sessions
ssl_session_tickets off;

ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-CHACHA20-POLY1305;
ssl_prefer_server_ciphers on;

# Optional settings:

# OCSP stapling
# ssl_stapling on;
# ssl_stapling_verify on;
# ssl_trusted_certificate /etc/letsencrypt/live/<your-nc-domain>/chain.pem;

# replace with the IP address of your resolver
# resolver 127.0.0.1; # needed for oscp stapling: e.g. use 94.140.15.15 for adguard / 1.1.1.1 for cloudflared or 8.8.8.8 for google - you can use the same nameserver as listed in your /etc/resolv.conf file

}
```



### Case study

So on my host machine, which for me is Ubuntu 20.04 and with Docker installed, I set up the following docker compose file in my home folder:

`docker-compose.yml` Read on before running this please ;)

```
version: "3"
services:
  nextcloud:
    container_name: nextcloud-aio-mastercontainer
    image: nextcloud/all-in-one:latest
    restart: always
    ports:
      - 8080:8080
    environment:
      - APACHE_PORT=11000
    volumes:
      - nextcloud_aio_mastercontainer:/mnt/docker-aio-config
      - /var/run/docker.sock:/var/run/docker.sock:ro
  nginx:
    container_name: nginx
    image: nginx:1.21.6
    restart: always
    ports:
      - 80:80
      - 443:443
    volumes:
      # - ./nginx:/etc/nginx
      - /etc/letsencrypt:/etc/letsencrypt

volumes:
  nextcloud_aio_mastercontainer:
    name: nextcloud_aio_mastercontainer
```

Now I need to draw your attention to the two volumes I've set up there. The first one is the nginx configuration folder, and it's commented out. Because unless you just so happen to have an nginx folder sitting on your host machine in the same folder as this file, nginx is just going to get upset by the lack of, well, anything to work with. We'll sort that in a second though, don't worry :)

The second one is the let's encrypt folder. This is where I have my SSL certificates that we absolutely need because reverse proxy mode requires a working SSL connection as it uses the address `https://yourdomain.tld` to connect.

\_So if you're like me, and you don't have your own certificates (don't rely on cloudflare's ones!), let's get some installed. On your hostmachine, install the "certbot" to get yourself a letsencrypt certificate. To do that, follow the instructions for your setup here: [https://certbot.eff.org/instructions](https://certbot.eff.org/instructions)

Once installed, just go ahead and run it, but let it know we're not actually all set up yet, so it doesn't need to try and install things for us, only give us our shiny new certificate :)\
For me, that's `sudo certbot certonly`\
Now you should have a nice new certificate. Assume you're using linux like me, it's probably been plonked inside `/etc/letsencrypt/live/yourdomain.tld/blah blah names and things`\
If it's in a different location, go up and change that line in the docker compose file. Note that the left hand side is the location of letsencrypt on your host machine, so that's the side you should change.\
Note also, and this is important, that you must bind the letsencrypt folder, **not** the live folder. It's a symlink. I won't get into it, but it's a pain in the arse.\_

Okay, so let's run it! Leave the nginx volume commented out, don't worry about that yet.\
`sudo docker up`\
If all runs well, you should see all those nice containers up and running without issue. If you're happy, hit `[control]+[Z]` to leave the process. Don't worry, the containers will stay running (unlike if you were to use `[control]+[C]`, which would stop them).

Now then, we want to get that nginx folder out of the running container and onto our host machine. Let's do that!

`sudo docker cp nginx:/etc/nginx ./nginx`

Excellent! Now you can uncomment that volume line:\
Change: `# - ./nginx:/etc/nginx` to just `- ./nginx:/etc/nginx`

And recreate the container by just running `sudo docker up` again. If you run `sudo docker up -d` you won't need to do `[control]+[Z]` to get out. But honestly, I prefer not to as I'm more likely to spot startup errors this way :)

Right then, now we can just configure nginx.

Open up your newly copied `./nginx/conf.d/default.conf` file in your preferred text editor.

You can go ahead and edit this file into a reliable state, or if you prefer, just use what I've ended up on (for the time being):

`./nginx/conf.d/default.conf`

```
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}

server {
    listen      443 ssl;
    server_name cloud.barratt.scot;
    location / {
        proxy_pass http://192.168.8.3:11000; #the host ip address, localhost I assume would just come back to this docker container
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        client_max_body_size 0;

        # Websocket
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }
    ssl_certificate /etc/letsencrypt/live/yourdomain.tld/fullchain.pem; # managed by certbot on host machine
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.tld/privkey.pem;
}
```

Note here that I've kept the default landing page on port 80, I just figured I wanted something there so I could check if the proxy was running etc.

I've also set the IP address for my host machine. For some reason I couldn't just use the host machine's host name, I'm not sure why. But make sure you change my `192.168.8.3` to whatever the correct IP is for your machine. (I'd recommend configuring your router to force the machine to always use the same IP address)

Note you'll also need to change `yourdomain.tld` to whatever domain your certbot certificate was made for :)

Now if you try to run the above, it'll fail with a message saying the variable named connection\_upgrade does not exist.

So let's fix that by now editing the nginx.conf file that you will have copied as part of the rest of the folder.

`./nginx/nginx.conf`

```

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    

    keepalive_timeout  65;

    #gzip  on;

    ##
    # Connection header for WebSocket reverse proxy
    ##
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    include /etc/nginx/conf.d/*.conf;
}
```

The only thing I've changed here is to add the block near the end, the bit that says

```
    ##
    # Connection header for WebSocket reverse proxy
    ##
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }
```

Now if you run `sudo docker compose up` you should see the nginx container recreate with the new stuff inside it as the new binding for the volume is linked up.\
And if you were to just make changes to this config, you can just run `sudo docker restart nginx` instead.

Hopefully that should be you all sorted :)

I hope to tidy this up a bit so that less editing is involved as an out of the box solution would be nicer. I'm thinking I'll try and put the certbot part into a docker container of its own for instance as I do like the portability of docker. I also think there's probably a way to avoid using the IP address of the host machine.
