# Pre-requirement for Docker

## INSTALLING DOCKER

So here we go…

`curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh`

After a looong time hopefully you will have installed docker, now let’s do some last tweaks.

#### CREATE A DOCKER GROUP

`sudo groupadd docker`

#### ADD YOUR USER TO THE DOCKER GROUP YOU JUST CREATED

`sudo gpasswd -a pi docker`
