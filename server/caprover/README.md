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



### Update an APP with a new image (after commit)

After updating the source image with a commit (please see [Broken link](broken-reference "mention")), open "Deployment" tab in Caprover App dashboard. Use "**Method 6: Deploy via ImageName"** and write the Docker Hub name (e.g. pasmer/nextcloud:update) and deploy it (press on Deploy Now).


