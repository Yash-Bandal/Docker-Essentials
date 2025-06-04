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
### 4.1 List all Volumes

```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     6a5178a7ba9c1b3a2fcd65713efd726ffaca871de6920ff0e9dfc3cdabde8 (anonymous)
local     64c0a692472e7c0479b20cd9587ff3f349f08a665fba9b088f0b3f4c3e741
```
displays all volumes

<br>

### 4.2 Create a Custom Named Volume (default `anonymous` Volumes)

```bash
docker volume create VOL_NAME
```
```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     6a5178a7ba9c1b3a2fcd65713efd726ffaca871de6920ff0e9dfc3cdabde8 (anonymous)
local     64c0a692472e7c0479b20cd9587ff3f349f08a665fba9b088f0b3f4c3e741
local     myVolume (named)
```

Creates a named volume, which our system allocates space automatically

They are saved default within :

---
> Windows : C:\ProgramData\docker\Volumes <br>
> Max/Linux :  /var/lib/docker/volumes
---
You can visit them and check

<br>




### 4.3 Remove Volume

```bash
docker volume rm VOL_NAME
```
```bash
PS E:\Docker\docker-testapp> docker volume rm myVolume
myVolume
```

Deletes the specified volume.

<br>

The named volumes we create are not attached with any `Container` by default and are isolated inside /docker/volumes <br>
But the main role of volume is to maintain persistancy in containers , for this we use different `flags`
```bash
--volume flag /-v
--mount flag
```

Mounting
1. Named Volumes
   ```bash
   docker run -v VOL_NAME: CONT_DIR_PATH
   ```
   If name Exits , its a Volume space is created with the name, or automatically a anonymous name is assiged <br>
   Named Volumes are prefered in real systems

<br>

2. Anonymous Volumes (No name ) - `tmpf`
   ```bash
   docker run -v  CONT_DIR_PATH
   ```
<br>

3. Bind Mounts
   ```bash
   docker run -HOST_PATH : CONT_DIR_PATH
   ```
The suttle difference between Named and anonumous (1,2) and Bind mounts (3) is that 
- ` Bind Mounts` are managed by **host machines**
- `Named` and `Anonymous` Volume are managed by **Docker** itself

  We can choose any volumes, but we prefer to choose as per use , same work can be done by `--mount` <br>
  [See mount syntax](https://docs.docker.com/engine/storage/volumes/)

<br>

## Docker Volume Prune
Sometimes, unused local volumes take up space. You can clean them using:
```bash
docker volume prune
```
Generally it targets anonymous volumes

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

# 7. Docker Exec and Logs<br> (Trouble shooting)
### 7.1 View Container Logs (Manually - Click on the `container-name ` and LOGS are visible)

```bash
docker logs <container_id>
```

Shows logs generated by a running container.

```bash
PS C:\Users\admin> docker logs 63bd10e05a89769d075e308269708b9cd03a36697910a276f8e5b328b772b3be

2025-06-03 02:53:08+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.3.0-1.el9 started.
2025-06-03 02:53:11+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2025-06-03 02:53:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.3.0-1.el9 started.
2025-06-03 02:53:11+00:00 [Note] [Entrypoint]: Initializing database files
.
.
```

<br>

### 7.2 Execute Command in Running Container
Allows to run additional commands inside container
#### A. /bash
```bash
docker exec -it <container_id> /bin/bash
```

OR if /bash terminal absent use

#### B. /sh
```bash
docker exec -it <container_id> /bin/sh
```
Now you can Runs a command inside a running container.
eg. ls, checking environment vars, 

```bash
bash-5.1# ls

bash-5.1# mkdir YASH_DIRECTORY

bash-5.1# ls
YASH_DIRECTORY  bin   dev                         etc   lib    media  opt   root  sbin         
ys  usr
afs             boot  docker-entrypoint-initdb.d  home  lib64  mnt    proc  run   srv                    
mp  var
```

Checking `environment variables` with `env`

```bash
bash-5.1# env
MYSQL_MAJOR=innovation
HOSTNAME=63bd10e05a89
PWD=/
MYSQL_ROOT_PASSWORD=secret
HOME=/root
MYSQL_VERSION=9.3.0-1.el9
GOSU_VERSION=1.17
TERM=xterm
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MYSQL_SHELL_VERSION=9.3.0-1.el9
_=/usr/bin/env
```
Exiting from bash (Not container)
```bash
bash-5.1#  exit
exit
```





<br>

---

<br>

# 8. Docker Compose<br>

### 8.1 Start Services

```bash
docker-compose up
```
`up` is an action that means to create and start the containers

Starts all services defined in `docker-compose.yml`.

<br>

### 8.2 Start Services in Background

```bash
docker-compose up -d
```
Runs services in detached mode. <br>
eg:
```bash
docker compose -f mongodb.yaml up -d
```




<br>

### 8.3 Stop Services

```bash
docker-compose down
```
eg:
```bash
docker compose -f mongodb.yaml down
```

Stops and removes containers, networks, and volumes defined in the Compose file.

---
> Note : to run the docker compose commands, open terminal in the required App `rg cd test-app`

> Place the `yaml` file outside public folder, external directory
---

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
