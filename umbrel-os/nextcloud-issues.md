# Nextcloud issues

## Infinite login loop

I was able to solve the issue by adding 'overwriteprotocol' => 'https' to my config.php file. This forces it to use https since it doesn't actually know it's being set to https via the reverse proxy.

For a perminant solution, Nextcloud would need to add support for the X-Forwarded-Proto header in the Reverse Proxy configuration (Nginx).

