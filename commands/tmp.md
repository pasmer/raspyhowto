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
