# TMP

To be reviewd



```

install.packages("remotes")
remotes::install_github("rspatial/terra")
remotes::install_github("r-spatial/mapview")
remotes::install_github("ropensci/rnaturalearthhires")


remotes::install_github("r-spatial/sf")
remotes::install_github("r-spatial/leafpop")

remotes::install_github("r-spatial/sf", configure.args = "--with-proj-lib=/usr/lib/grass76/etc/proj")
/usr/lib/grass76/etc/proj

pkg-config --libs proj

# list all packages where an update is available
old.packages()

# update all available packages
update.packages()

# update, without prompts for permission/clarification
update.packages(ask = FALSE)



rmarkdown::render('file.Rmd', output_file='file.html')


rmarkdown::render('file.Rmd', output_file='file.pdf')

```

Prove



```
docker run -v /path/to/your/roms:/roms -p 80:80 pasmer/retroarch


captainVersion: 4
services:
    $$cap_appname:
        image: pasmer/retroarch:latest
        restart: always
        environment:
            USER: 'root'
            PASSWORD: 'Skynet'
        ports:
            - '8777:80'
        volumes:
            - $$cap_appname-roms:/roms
        caproverExtra:
            containerHttpPort: '80'
caproverOneClickApp:
    instructions:
        start: Inizio
        end: Fine
    description: Test descrizione


ENV USER=root
ENV PASSWORD=password1
```

### Test Nginx Proxy Manager configuration

```
version: '3.8'
services:
  app:
    image: 'yobasystems/alpine-mariadb:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '8050:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP
    environment:
      # Mysql/Maria connection parameters:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
      MARIADB_AUTO_UPGRADE: '1'
    volumes:
      - ./mysql:/var/lib/mysql
```

LINK:

{% embed url="https://nginxproxymanager.com/" %}
