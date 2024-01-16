---
description: Install on ARM64 - from Andrés Castro Socolich's Blog @Andresrcs
---

# R Server & Shiny

### Install R

For installing R in your Pi you have some options, each one with its own pros and cons:

* The standard deb repositories for your operating system provide R binaries that are very simple to install but those are old R versions (4.0.0 for Raspberry Pi OS and 4.1.2 for Ubuntu 22.04.1).

```
sudo apt install r-base
```

* The [R4Pi Project](https://r4pi.org/) provides a deb repository for precompiled 32-bit binaries for R since 2021and 64-bit binaries since September 2022, installing R from these precompiled binaries is much faster than compiling yourself, and they also provide a package-repository with precompiled binaries which makes packages installation way faster and easier.

```
curl -O  https://pkgs.r4pi.org/dl/r4pi-repo-conf_0.0.1-1_all.deb
sudo dpkg -i  r4pi-repo-conf_0.0.1-1_all.deb
sudo apt update
sudo apt upgrade
sudo apt install r4pi
rm r4pi-repo-conf_0.0.1-1_all.deb
```

Sadly, they are compatible only with Raspberry Pi OS, if you have chosen this operating system, I strongly recommend using this option since the speed gains are considerable.

* You can always compile R from source regardless of the operating system you are using, this offers you the opportunity to choose compilation options but at the expense of waiting a long time for the compilation process to complete.

```
curl -O  https://pkgs.r4pi.org/dl/r4pi-repo-conf_0.0.1-1_all.deb
sudo dpkg -i  r4pi-repo-conf_0.0.1-1_all.deb
sudo apt update
sudo apt upgrade
sudo apt install r4pi
rm r4pi-repo-conf_0.0.1-1_all.deb
```

You can always compile R from source regardless of the operating system you are using, this offers you the opportunity to choose compilation options but at the expense of waiting a long time for the compilation process to complete.

```
# Set the R version to install
R_VERSION=4.2.1

sudo apt install -y g++ gcc gfortran libreadline-dev libx11-dev libxt-dev \
                    libpng-dev libjpeg-dev libcairo2-dev xvfb \
                    libbz2-dev libzstd-dev liblzma-dev libtiff5 \
                    libssh-dev libgit2-dev libcurl4-openssl-dev \
                    libblas-dev liblapack-dev libopenblas-base \
                    zlib1g-dev openjdk-11-jdk \
                    texinfo texlive texlive-fonts-extra \
                    screen wget libpcre2-dev make cmake
cd /usr/local/src
sudo wget https://cran.rstudio.com/src/base/R-${R_VERSION:0:1}/R-$R_VERSION.tar.gz
sudo su
tar zxvf R-$R_VERSION.tar.gz
cd R-$R_VERSION
./configure --enable-R-shlib --with-blas --with-lapack #BLAS and LAPACK are optional
make
make install
cd ..
rm -rf R-$R_VERSION*
exit
```

### Install Shiny Server

Currently, `shiny-server` offers no ARM support so the only option for installing it on the Pi is to compile from source. There is hope this will change in the future as per the response on this [GitHub Issue](https://github.com/rstudio/shiny-server/issues/532#issuecomment-1223282001), but for the time being we are stuck with this.

First, you need to install the R package dependencies, if you are using a CRAN repository, this is going to take a while, but it is much quicker if you are using the R4Pi repository (available with R installed from R4Pi):

```
# Define your R package repository
REPO="'http://cran.rstudio.com/'" # For R installed from a standard repository or compiled from CRAN
REPO="c('https://pkgs.r4pi.org/aarch64', 'http://cran.rstudio.com/')" # For R installed from R4Pi

# Make sure the system dependencies are installed
sudo apt install libcairo2-dev libxt-dev git cmake pandoc pandoc-citeproc

# Install required R packages as sudo
sudo su - -c "R -e \"install.packages('Rcpp', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('later', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('fs', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('R6', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('Cairo', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('httpuv', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('mime', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('jsonlite', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('digest', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('htmltools', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('xtable', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('sourcetools', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('shiny', repos=$REPO)\""
sudo su - -c "R -e \"install.packages('rmarkdown', repos=$REPO)\""
```

Now, you can install shiny-server with these commands:

```
# Download the source code for the latest shiny-server release from GitHub
git clone --depth 1 --branch v1.5.19.995 https://github.com/rstudio/shiny-server.git

# Compile the source code
cd shiny-server
DIR=`pwd`
PATH=$DIR/bin:$PATH
mkdir tmp
cd tmp
PYTHON=`which python`
sudo cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DPYTHON="$PYTHON" ../
sudo make
mkdir ../build

# Modify some download links and SHASUMs for arm64 compatibility
sed -i '8s/.*/NODE_SHA256=5a6e818c302527a4b1cdf61d3188408c8a3e4a1bbca1e3f836c93ea8469826ce/' ../external/node/install-node.sh # SHAMSUM for node-v16.14.0-linux-arm64.tar.xz
sed -i 's/linux-x64.tar.xz/linux-arm64.tar.xz/' ../external/node/install-node.sh
sed -i 's/https:\/\/github.com\/jcheng5\/node-centos6\/releases\/download\//https:\/\/nodejs.org\/dist\//' ../external/node/install-node.sh
sed -i 's/sha512-3RAVyfbptsR6HOFA0BFNLyw8ZXXDRWf5P3tIslbNt12kTikaRWepRR9vLHMyibIZeNfScI9uGqcn1KfbIAeuXA==/sha512-ZwrJM2WaOJesJGZlejLqAiBAE6Ts2PZNk1pQ\/x1uTMsQw83BaXWShjqCbhh5bPQUNrlx2Ijz1dOr0hLmlkxKag==/' ../npm-shrinkwrap.json

# Install node for arm64 and rebuild node modules
(cd .. && sudo ./external/node/install-node.sh)
(cd .. && ./bin/npm --python="${PYTHON}" install --no-optional)
(cd .. && ./bin/node ./ext/node/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js --python="${PYTHON}" rebuild)

# Install shiny-server
sudo make install

# Configure shiny-server
sudo mkdir -p /etc/shiny-server
sudo cp ../config/default.config /etc/shiny-server/shiny-server.conf
cd
sudo rm -rf shiny-server
sudo ln -s /usr/local/shiny-server/bin/shiny-server /usr/bin/shiny-server
sudo useradd -r -m shiny
sudo mkdir -p /var/log/shiny-server
sudo mkdir -p /srv/shiny-server
sudo mkdir -p /var/lib/shiny-server
sudo chown shiny /var/log/shiny-server

# Edit the shiny-server.service file
sudo nano /etc/systemd/system/shiny-server.service 
    # Paste the following content
    
    [Unit]
    Description=ShinyServer

    [Service]
    Type=simple
    ExecStart=/usr/bin/env bash -c 'exec /usr/local/shiny-server/bin/shiny-server >>     /var/log/shiny-server.log 2>&1'
    KillMode=process
    ExecReload=/usr/bin/env kill -HUP $MAINPID
    ExecStopPost=/usr/bin/env sleep 5
    Restart=on-failure
    RestartSec=1
    StartLimitInterval=45
    StartLimitBurst=3

    [Install]
    WantedBy=multi-user.target

# Edit the shiny-server logrotate file
sudo nano /etc/logrotate.d/shiny-server 
    # Paste the following content
    
    /var/log/shiny-server.log {
       rotate 12
       copytruncate
       compress
       missingok
       size 1M
    }

# Set permissions for the files
sudo chown root:root /etc/systemd/system/shiny-server.service
sudo chmod 644 /etc/systemd/system/shiny-server.service
sudo chown root:root /etc/logrotate.d/shiny-server
sudo chmod 644 /etc/logrotate.d/shiny-server

# Start the service
sudo systemctl daemon-reload
sudo systemctl enable shiny-server
sudo systemctl start shiny-server

# Create sylinks for pandoc and the sample apps
sudo ln -s -f /usr/bin/pandoc /usr/local/shiny-server/ext/pandoc/pandoc
sudo ln -s -f /usr/bin/pandoc-citeproc /usr/local/shiny-server/ext/pandoc/pandoc-citeproc
sudo ln -s /usr/local/shiny-server/samples/sample-apps /srv/shiny-server/sample-apps
sudo ln -s /usr/local/shiny-server/samples/welcome.html /srv/shiny-server/index.html

# Set proper user permissions, I'm assuming your user is "pi", change it if it isn't
sudo groupadd shiny-apps
sudo usermod -aG shiny-apps pi
sudo usermod -aG shiny-apps shiny
cd /srv/shiny-server
sudo chown -R pi:shiny-apps .
sudo chmod g+w .
sudo chmod g+s .
cd
```

### Install Rstudio Server

In the past, the only way to install RStudio Server on a Pi was to compile from source and this was extremely time-consuming (took me 3 days the last time I tried on a Pi 3B+) and very prone to failure because the RStudio IDE is under active development.

Luckily, in recent days, RStudio has made available “experimental” builds of the IDE for arm64 (I wrote an [article about it](https://andresrcs.rbind.io/2022/08/22/rstudio\_ide\_arm/)), they are compiled for Ubuntu 18/20 and 22 but the Ubuntu 18/20 are compatible with Raspberry Pi OS as well. You can check the available daily builds [here](https://dailies.rstudio.com/rstudio/elsbeth-geranium/). Choose a link accordingly to the operating system you are using, also, have in mind that these builds are meant for testing so they are likely to have bugs or be unstable, if you have problems installing the latest one available, try with an older version.

The following instructions use a link for Ubuntu 18/20 (Raspberry Pi OS compatible), remember to change the link if you want to use another OS or a more recent IDE version:

```
sudo apt install gdebi
wget https://s3.amazonaws.com/rstudio-ide-build/server/bionic/arm64/rstudio-server-2022.11.0-daily-164-arm64.deb
sudo gdebi rstudio-server-2022.11.0-daily-164-arm64.deb
sudo rm rstudio-server-2022.11.0-daily-164-arm64.deb
```

### Extra Steps (optional)

Make pretty URLs for Shiny and Rstudio Server (e.g. `http://your-ip/shiny/your-app` instead of `http://your-ip:3838/your-app`) by using Nginx as a reverse proxy, you only need to replace the content this file, `sudo nano /etc/nginx/sites-enabled/default`, with this:

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /var/www/html;
    
    index index.html index.htm;
    
    server_name _;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    rewrite ^/shiny$ $scheme://$http_host/shiny/ permanent;
    
    location /shiny/ {
        rewrite ^/shiny/(.*)$ /$1 break;
        proxy_set_header    Host $host;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto $scheme;
        proxy_pass          http://localhost:3838;
        proxy_read_timeout  20d;
        proxy_buffering off;
        
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_http_version 1.1;
        
        proxy_redirect      / $scheme://$host/shiny/;
    }
    
    location /rstudio/ {
        rewrite ^/rstudio/(.*)$ /$1 break;
    
        proxy_pass http://localhost:8787;
    
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_read_timeout 20d;
        proxy_buffering off;
    
        proxy_set_header X-RStudio-Root-Path /rstudio;
    
        proxy_set_header Host $host:$server_port;
    }
}
```

On Raspberry Pi OS there is a weird issue where Nginx starts before RStudio Server is able to start, and the reverse proxy fails. A walk-around for this is to edit this file, `sudo nano /etc/systemd/system/multi-user.target.wants/nginx.service`, and replace the part that says `After=network.target` with `After=network-online.target` and reload the daemon with `sudo systemctl daemon-reload`. You can omit this if you are using Ubuntu.

Now you have to restart Nginx to apply the changes:

```
sudo systemctl restart nginx
```
