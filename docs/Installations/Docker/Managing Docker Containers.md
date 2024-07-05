  

 **Introduction:** 

Docker provides the ability to **package** and **run an application** in an isolated environment called a **container**. Container management is a process for automating the creation, deployment and scaling of containers. 

Container management facilitates the addition, replacement and organization of containers on a large scale


1. Lets list the container if any container is running 
```
docker container ls -a
```

2.   **Create** a nginx container
```
 docker container create nginx
```
3.   **create** a container and **name** it as ofl01 
```
docker container create --name ofl01 nginx
```
   4. **Start** the container which is created
```
docker container start ofl01
```
   5. Create a new container using run option
```
docker container run --name server01 centos
```
   6. Create a new container by supplying (-i and -t) option to get interactive terminal, by executing the below command, Note: “**-i**” stands for interactive & “**-t**” stands for terminal. So we are requesting an interactive terminal for a container.
```
docker container run --name server02 -i -t centos
```

	**Note:** **The container is created and interactive terminal is displayed. You can run commands inside the container and exit out.**

7. Create a new container with detached interactive terminal (-d -i -t) option, by executing the below command.
	**Note:** **d** **stand detached mode.**
```
docker container run --name server03 -dit centos
```
8.   **Attach** the container server03
```
docker container attach server03
```
9. Run a command inside the container without attaching to the container by using exec option.
```
docker container exec server03 cat /etc/resolv.conf
```
10. **Rename** the existing container
```
docker container rename server03 server003
```
11. **Pause** the existing container
```
docker container pause server003
```
12. **Unpause** the existing container
```
docker container unpause server003
```
13. **Stop** the existing container
```
docker container stop server003
```
14. **Start** the existing container 
```
docker container start server003
```
16.  **Restart** the existing container
```
docker container restart server003
```
17. **Kill** the container
```
docker container kill server003
```
