# CapRover

Schema version

```
{
  "schemaVersion": 2,
  "dockerfileLines": [
    "FROM library/python:3.6.3-alpine",
    "RUN apk update && apk upgrade && apk add --no-cache make g++ bash git openssh postgresql-dev curl",
    "RUN mkdir -p /usr/src/app",
    "WORKDIR /usr/src/app",
    "COPY ./django_project/ /usr/src/app",
    "RUN pip install -r requirements.txt",
    "COPY ./utils/ /usr/src/utils",
    "EXPOSE 80",
    "CMD sh /usr/src/utils/run.sh"
  ]
}
```



### New App using Template

Use this for Shiny-Server:

```
captainVersion: 4
caproverOneClickApp:
    instructions:
        start: Just a plain Docker Compose.
        end: Docker Compose is deployed.
########
version: '3.3'
services:
  $$cap_appname:
    image: hvalev/shiny-server-arm:latest
    ports:
      - 3838:3838
    caproverExtra:
      containerHttpPort: '3838'
    volumes:
       - $$cap_appname-apps:/srv/shiny-server/
       - $$cap_appname-logs:/var/log/shiny-server/
       - $$cap_appname-conf:/etc/shiny-server/
    restart: always
```

### Update an APP with a new image (after commit)

After updating the source image with a commit (please see [Broken link](broken-reference "mention")), open "Deployment" tab in Caprover App dashboard. Use "**Method 6: Deploy via ImageName"** and write the Docker Hub name (e.g. pasmer/nextcloud:update) and deploy it (press on Deploy Now).



