# Reverse Proxy

Template&#x20;

```
http {
  tcp_nopush on;
  tcp_nodelay on;

  upstream droppy {
    server 127.0.0.1:8989;
  }

  server {
    listen 80;
    listen [::]:80;

    server_name droppy.example.com;
    return 301 https://$host$request_uri;
  }

  server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name droppy.example.com;
    access_log /var/log/nginx/droppy.example.com.log;

    ssl_certificate /etc/letsencrypt/live/droppy.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/droppy.example.com/privkey.pem;
    ssl_trusted_certificate /etc/letsencrypt/live/droppy.example.com/fullchain.pem;
   
    location / {
      proxy_pass http://droppy/;
      proxy_set_header Host $host;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $http_connection;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Real-Port $remote_port;
      proxy_http_version 1.1;
      proxy_cache off;
      proxy_buffering off;
      proxy_redirect off;
      proxy_request_buffering off;
      proxy_ignore_client_abort on;
      proxy_connect_timeout 7200;
      proxy_read_timeout 7200;
      proxy_send_timeout 7200;
      client_max_body_size 0;
    }
  }
}
```
