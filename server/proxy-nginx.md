# Proxy Nginx

<mark style="color:red;">**UNDER REVIEW**</mark>

## Container Docker - CapRover

Add websocket support on Nginx conf file in the Proxy server (main host).

Add this lines in the server backets:

```
// Websocket support
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_http_version 1.1;
```
