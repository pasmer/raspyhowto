# Shiny Server (docker)

Easy install Shiny Server using CapRover App through docker image (64bit needed).

Follow those steps:

1. Create a new APP using CapRover
2. Deploy using this docker image:

```
hvalev/shiny-server-arm:latest
```

3. Create new Volumes for the APP folder (to be shared with Rserver)



### Pandoc problem (how to solve)

Access to the docker terminal and use this:

```
rm /usr/local/shiny-server/ext/pandoc/pandoc
ln -s /usr/bin/pandoc /usr/local/shiny-server/ext/pandoc/pandoc
```
