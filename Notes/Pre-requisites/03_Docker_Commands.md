<h1 align = "center">  Docker Major Commands </h1>

This lesson covers all major Docker commands, grouped into logical categories, with brief explanations and formatted outputs.

Type `docker` to get list of all commands in terminal
```
docker
```

Some major commands used are :

<br>

# 1. Docker Version and Info<br>

### 1.1 Show Docker Version

```bash
docker version
```

Displays client and server Docker version info.

<br>

### 1.2 Show System-wide Docker Info

```bash
docker info
```

Shows information about Docker installation, containers, images, and more.

<br>

---

<br>

# 2. Docker Images<br>

### 2.1 List Docker Images

```bash
docker images
```

Lists all locally available Docker images.

<br>

### 2.2 Pull Image from Docker Hub
```
docker pull IMAGE_NAME : VERSION
```

#### 2.3.1 Default Pull Latest Image
```bash
docker pull mysql
```
#### 2.3.2 Specific Version
```bash
docker pull mysql:8.0
```

Downloads the mysql image from Docker Hub.
- This allows to use 2+ versions of a IMAGE in different containers

<br>

### 2.3 Remove Image

```bash
docker rmi <image_id>
```

Deletes the specified image from the local system.

<br>

---

<br>

# 3. Docker Containers<br>

### 3.1 Run a New Container

```bash
docker run ubuntu
```

Runs a container using the Ubuntu image.

<br>

### 3.2 Run with Interactive Terminal 🚩

```bash
docker run -it ubuntu /bin/bash
```

- Launches an interactive shell session inside the Ubuntu container, that allow input and output.
- Allows to enter a container  having its own `individual environment`,  where you can run CLI commands like `ls` and all
- Now you can create your own `Directories` within this container
```bash
mkdir YB-Productions
```
```
root@b9be8f931b5d:/# ls
YB-productions  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

<br>

### 3.3 Run in Detached Mode 

```bash
docker run -d nginx
```

Runs the container in background (detached) mode.

<br>

### 3.4 List Running Containers

```bash
docker ps
```
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Shows currently running containers.

<br>

### 3.5 List All Containers

```bash
docker ps -a
```
```
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                          PORTS     NAMES
b9be8f931b5d   ubuntu        "/bin/bash"   2 minutes ago    Exited (0) About a minute ago             vigorous_gauss
04eb33e209de   hello-world   "/hello"      50 minutes ago   Exited (0) 50 minutes ago                 festive_yonath
```

Shows all containers including stopped ones.

<br>

### 3.6 Start a Running Container 

>..no <>

```bash
docker start <container_name> OR <container_id>
```

Gracefully starts  a  existing running container.
- Green Light starts inside dashboard 🟢

<br>

### 3.7 Stop a Running Container

```bash
docker stop <container_name> OR <container_id>
```

Gracefully stops a running container. 
- Green Light stops inside dashboard ⚫

<br>

### 3.8 Remove a Container

```bash
docker rm <container_id>
```

Deletes a stopped container.
- If we try running container
  
  ```
   > Error response from daemon: conflict: unable to delete 6015f66923d7 (cannot be forced) - container is being used by running
  ```

<br>

### 3.9 Remove a Image

```bash
docker rmi <IMAGE_id>
```

Deletes a stopped IMAGE.
- If we try to delete IMAGE with running container
- First we need to remove container -> then Image

```bash
1. Error
PS C:\Users\admin> docker rmi  0b6a027b5cf3
Error response from daemon: conflict: unable to delete 0b6a027b5cf3 (cannot be forced) - image is being used by running container 0b6a027b5cf3

2. delete container
PS C:\Users\admin> docker rm 04eb33e209de
04eb33e209de

3. delete image
PS C:\Users\admin> docker rmi   0b6a027b5cf3
Untagged: hello-world:latest
Deleted: sha256:0b6a027b5cf322f09f6706c754e086a232ec1ddba835c8a15c6cb74ef0d43c29
```

<br>

---

<br>

# 4. Docker Volumes<br>

### 4.1 Create a Volume

```bash
docker volume create mydata
```

Creates a named volume.

<br>



### 4.2 List Volumes

```bash
docker volume ls
```

Displays all Docker volumes.

<br>

### 4.3 Remove Volume

```bash
docker volume rm mydata
```

Deletes the specified volume.

<br>

---

<br>

# 5. Docker Networks<br>

### 5.1 List Networks

```bash
docker network ls
```

Shows all Docker networks.

<br>

### 5.2 Create Network

```bash
docker network create mynet
```

Creates a new user-defined bridge network.

<br>

### 5.3 Connect Container to Network

```bash
docker network connect mynet <container_id>
```

Connects a container to a specific network.

<br>

---

<br>

# 6. Dockerfile and Build<br>

### 6.1 Build Image from Dockerfile

```bash
docker build -t myimage .
```

Builds a Docker image using the Dockerfile in the current directory.

<br>

---

<br>

# 7. Docker Exec and Logs<br>

### 7.1 Execute Command in Running Container

```bash
docker exec -it <container_id> /bin/bash
```

Runs a command inside a running container.

<br>

### 7.2 View Container Logs

```bash
docker logs <container_id>
```

Shows logs generated by a running container.

<br>

---

<br>

# 8. Docker Compose<br>

### 8.1 Start Services

```bash
docker-compose up
```

Starts all services defined in `docker-compose.yml`.

<br>

### 8.2 Start Services in Background

```bash
docker-compose up -d
```

Runs services in detached mode.

<br>

### 8.3 Stop Services

```bash
docker-compose down
```

Stops and removes containers, networks, and volumes defined in the Compose file.

<br>

---

# Exit Container :
```
exit
```

<br>

##  Summary Table<br>

| Command                  | Description                 |
| ------------------------ | --------------------------- |
| `docker run`             | Start a new container       |
| `docker ps`              | List containers             |
| `docker images`          | List images                 |
| `docker exec`            | Run commands in containers  |
| `docker logs`            | View logs                   |
| `docker build`           | Build image from Dockerfile |
| `docker volume`          | Manage volumes              |
| `docker network`         | Manage networks             |
| `docker-compose up/down` | Manage multi-container apps |


<br>

[MYSQL_image](https://hub.docker.com/_/mysql)

---
> These Docker commands form the foundation of container-based development and deployment workflows.
---
