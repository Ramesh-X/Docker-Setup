# Setup docker environment Ubuntu 16.04

## Installing
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
```

Run `apt-cache policy docker-ce` and the output will look like
```
docker-ce:
  Installed: (none)
  Candidate: 17.03.1~ce-0~ubuntu-xenial
  Version table:
     17.03.1~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     17.03.0~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

Notice that _docker_ is **not installed** and _download location_ is **download.docker.com**
Now you can install docker
```
sudo apt-get install -y docker-ce
sudo systemctl status docker
```
<b> * Make sure the docker is active and running before continue </b>

## Execute docker without `sudo`

```
sudo groupadd docker
sudo gpasswd -a $USER docker
```
And type  `docker run hello-world` to make sure _docker_ running without `sudo`

## Working with docker images
`docker run hello-world`
Check whether docker run properly

`docker search ubuntu`
Search docker images in **Docker Hub** which has `ubuntu` in their docker name.

`docker pull ubuntu`
Download the _docker_ named _ubuntu_ to the local machine

`docker run ubuntu`
Run a docker container with the `ubuntu` docker image. If it is not pulled, the imaged will be pulled before run.

`docker images`
Show all docker images in the local machine

## Running a Docker Container
`docker run -it --name cotainer_name ubuntu`
Run a docker container with interactive shell. The output will be like:
`root@d9b100f2f636:/#`

You can now update the container and install something.
```
apt-get update
apt-get install vim
```

## Commit Changes in a Container to a Docker Image
First exit from the container with `exit` command. Then run the following.

```
exit
docker commit -m "What did you do to the image" -a "Author Name" container-id  repository/new_image_name
```

For example:
```
docker commit -m "initial commit" -a "rameshpr" d9b100f2f636 video-ml/rtpose
```

Run `docker images` and see the new image is there.


## Listing Docker Containers

`docker ps`
List all active docker containers

`docker ps -a`
List all active and inactive docker containers

`docker ps -l`
View status of the latest container that you created

`docker stop container-id`
Stop the active container with `container-id`. It can be found using `docker ps`

## Run stopped docker container

```
docker start 6ed287ad073b
docker attach 6ed287ad073b
```
This start the stopped container and attach the terminal to it.

## Remove docker containers

`docker ps -a -f status=exited`
View all exited docker containers

`docker rm $(docker ps -a -f status=exited -q)`
remove all exited docker containers

## Copy files from Container
`docker cp <containerId>:/file/path/within/container /host/path/target`
Here the `containerId` can be the _name_ or the _ID_ of the container which is already started.

**Guide on removing docker _images_, _containers_ or _volumes_** 
https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes

## Change docker image and container location
https://sanenthusiast.com/change-default-image-container-location-docker/

If you want to change the size of the docker storage drive you can replace this line in `docker.conf` file.
```
ExecStart=/usr/bin/dockerd --graph="/mnt/new_volume" --storage-driver=devicemapper --storage-opt dm.basesize=20G
```
Then restart the docker.


## More information
1. https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04
2. https://askubuntu.com/a/477554/568872
