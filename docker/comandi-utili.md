---
cover: >-
  https://images.unsplash.com/photo-1552664730-d307ca884978?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2970&q=80
coverY: 0
---

# Comandi utili

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



### Upload an image (after commit) to Docker Hub

First things first, login to Docker:

```
sudo docker login --username email@server.com --password XXXXXXXXXX
```

A repository on your Docker Hub must be created, I assume that is "pasmer/nextcloud". Here the command to upload the new image (with the following tag: "update") from terminal:

```
sudo docker push pasmer/nextcloud:update
```

