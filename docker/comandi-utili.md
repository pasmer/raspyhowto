---
cover: >-
  https://images.unsplash.com/photo-1552664730-d307ca884978?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2970&q=80
coverY: 0
---

# Useful commands

## Guide

{% embed url="https://www.mygreatlearning.com/blog/top-essential-docker-commands/" %}

## List of commands

Let’s explore some of the most commonly used Docker commands that can help you manage Docker containers effectively.

1. `docker run` – This command is used to start a new Docker container from an image.
2. `docker ps` – This command is used to list all the running Docker containers.
3. `docker stop` – This command is used to stop a running container.
4. `docker rm` – This command is used to remove a Docker container.
5. `docker images` – This command is used to list all the Docker images that are currently available on your system.
6. `docker pull` – This command is used to download a Docker image from a registry.
7. `docker exec` – This command is used to execute a command in a running container.
8. `docker-compose` – This command is used to manage multi-container Docker applications.

## How to copy files to/from a container

*   **To copy a file from the local file system to a container**, run the command for Docker container or Kubernetes pod, respectively:

    ```
    docker cp <src-path> <container>:<dest-path> 
    ```

    For example:

    ```
    docker cp "C:\ProcDump\procdump64.exe" sitecore-xm1_cm_1:"C:\"
    ```
*   **To copy a file from the container to the local file system**, use:

    ```
    docker cp <container>:<src-path> <local-dest-path>
    ```

    For example:

    ```
    docker cp sitecore-xm1_cm_1:"C:\inetpub\wwwroot\App_Data\logs" "C:\" 
    ```

## Connect to the Container's terminal <a href="#ftoc-heading-5" id="ftoc-heading-5"></a>

This command is used to access the container that is running. Find the Container ID (docker ps -a) Here is how to use it:

```
docker exec -it CONTAINERID bash
mysql -uroot -pmy-secret-pw
SHOW DATABASES;
```

## Commit Changes to Image <a href="#ftoc-heading-5" id="ftoc-heading-5"></a>

Finally, create a new image by committing the changes using the following syntax:

```
sudo docker commit [CONTAINER_ID] [new_image_name]
```

Therefore, in our example it will be:

```
sudo docker commit deddd39fa163 ubuntu-nmap
```

Where **`deddd39fa163`** is the CONTAINER ID and **`ubuntu-nmap`** is the name of the new image.

To find the CONTAINER ID run this from the host terminal:

```
sudo docker ps
```

Find the ID corresponding to IMAGE name.



### Tagging During the Docker Image Build Process

Tagging a Docker image during the build process is straightforward. You can do this by using the `-t` option followed by the repository name and tag name when you run the `docker build` command. For example:

```bash
docker build -t myapp:1.0 .
```

BashCopy

In this command, `myapp` is the repository name and `1.0` is the tag name. The `.` at the end of the command tells Docker to build the image using the Dockerfile in the current directory.



### Tagging After the Docker Image Build Process

You can also tag a Docker image after it has been built. This is done using the `docker tag` command. For example:

```bash
docker tag image_id myapp:2.0
```

BashCopy

In this command, `image_id` is the ID of the image you want to tag, `myapp` is the repository name, and `2.0` is the tag name. You can find the image ID by using the [`docker images`](https://ioflood.com/blog/docker-list-images-easy-guide-to-docker-images-ls-command/) command, which lists all your Docker images along with their IDs.



### Upload an image (after commit) to Docker Hub

First things first, login to Docker:

```
sudo docker login --username email@server.com --password XXXXXXXXXX
```

A repository on your Docker Hub must be created, I assume that is "pasmer/nextcloud". Here the command to upload the new image (with the following tag: "update") from terminal:

```
sudo docker push pasmer/nextcloud:update
```

