
#### Install Docker Engine on RHEL

1. Uninstall the old versions CRI 
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine \
                  podman \
                  runc
                  
```

2. Docker Installation

	- 2.1 Setup the Repository 
```
sudo yum install -y yum-utils
```
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```
> Note: If you want to can check the repo status -->  `yum repolist`  


3. Install Docker
	- 3.1 Install latest version of docker
```
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```
> Note:   Install specific version of docker
```
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io docker-buildx-plugin docker-compose-plugin
```

4. Start Docker
```
sudo systemctl start docker
```
5. Enable docker
```
systemctl enable docker.service
```
6. Check docker status
```
systemctl status docker.service --no-pager
```
7. Verify that the Docker Engine installation is successful by running the `hello-world` image.
```
sudo docker run hello-world
```
```
docker container ls 
```
8. Docker Engine cleanup 
	- 8.1 How to remove  containers 
```
docker container stop $(docker container ls -aq)

docker container rm $(docker container ls -aq) 
```
	- 8.2 How to remove docker images 
```
docker rmi $(docker images -q) 
```