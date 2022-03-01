# Nginx and PHP



## PHP

Install the last version of PHP (used 8.0 in the examples).



Install the Modules (assuming PHP 8.0)

```
sudo apt install php8.0-gd php8.0-mysql php8.0-curl php8.0-mbstring php8.0-intl -y
sudo apt install php8.0-gmp php8.0-bcmath php-imagick php8.0-xml php8.0-zip -y
```

### Server Apache

A simple command for this on raspbian is `update-rc.d apache2 disable` If you later want the webserver starts again on default just type `update-rc.d apache2 enable`'

With the comand `sudo /etc/init.d/apache2 start` you bring up the web server on demand.

