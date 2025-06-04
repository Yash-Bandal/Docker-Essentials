# 10 Docker Compose
A tool for defining and running multi-container applications.  <br>
 So far, we’ve used Docker commands to set up and run multiple containers, and while that worked, Docker Compose simplifies the process even more.

## 1. Understand Why Use Docker Compose?
- Avoids repetitive and complex docker run commands that we make on the `Terminal`.
- It Manages configurations like:
    - Port binding
    - Container names
    - Networks
    - Environment variables
Thus , compose does All setup is written in a single `YAML (.yml)` file in a structured standardized way , as its very difficult to make changes via terminal commands.

<table>
<tr>
  <td>
    <img src="https://github.com/Yash-Bandal/Docker-Essentials/blob/80597dc58a4fdf6164c136349e801be82dc0af0c/Notes/StoreRoom/10_Cmd.PNG" width="400">
    <h3> Standard Terminal Command </h3>
  </td>
  <td>
    <img src="https://github.com/Yash-Bandal/Docker-Essentials/blob/80597dc58a4fdf6164c136349e801be82dc0af0c/Notes/StoreRoom/10_Yaml.PNG" width="400">
    <h3>Indented YAML File</h3>
  </td>
</tr>
</table>

- Indentation is very important in YAML files—always use proper spacing (tabs or two spaces) to maintain structure.
- By comparing this YAML file to long Docker CLI commands, we clearly see how much cleaner and easier Compose is.
- Editing things like usernames, passwords, or URLs in the YAML file is simple—just change a line, thus its Much easier to manage and maintain than using CLI commands for each container.

<br>

## 2. Create YAML files inside `APP`
```yml
version : "3.8"

services :  #containers and its requirements
  mongo : 
    image : mongo 
    ports : 
    - 27017:27017
    environment :
      MONGO_INITDB_ROOT_USERNAME : admin  
      MONGO_INITDB_ROOT_PASSWORD : qwerty

  mongo-express :
    image : mongo-express
    ports :
      -8081:8081
    environment :
      ME_CONFIG_MONGODB_ADMINUSERNAME : admin
      ME_CONFIG_MONGODB_ADMINPASSWORD : qwerty 
      ME_CONFIG_MONGODB_URL : mongodb://admin:qwerty@mongo:27017/ # remove "" and add / at end 
```

<br>

## 3. Check Docker Commands
[Docker Compose Commands](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/Pre-requisites/03_Docker_Commands.md#8-docker-compose)



## Network
We see that theres no such option of `--network` in `.yml` file, this is because, `yaml` builds a `default Network `beforehand and then run all containers within

<br>

## 4. Delete our previously created and pulled IMAGES and Running Containers

<br>

## 5. Open the test-app in terminal and run 
```bash
PS E:\Docker\docker-testapp> docker compose -f mongodb.yaml up -d
```
```bash
time="2025-06-04T14:42:50+05:30" level=warning msg="E:\\Docker\\docker-testapp\\mongodb.yaml: the attribute `version` is obsolete, it will be ig
nored, please remove it to avoid potential confusion"
[+] Running 18/18
 ✔ mongo Pulled                                                                                                                         1167.9s 
 ✔ mongo-express Pulled                                                                                                                  524.6s 
[+] Running 3/3
 ✔ Network docker-testapp_default            Created                                                                                       0.7s 
 ✔ Container docker-testapp-mongo-1          Started                                                                                       6.2s 
 ✔ Container docker-testapp-mongo-express-1  Started                                                                                       6.2s

PS E:\Docker\docker-testapp> docker ps -a
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                      NAMES
2cb5f51ab460   mongo-express   "/sbin/tini -- /dock…"   4 minutes ago   Up 5 minutes   0.0.0.0:8081->8081/tcp     docker-testapp-mongo-express-1
3c562b6e2cc8   mongo           "docker-entrypoint.s…"   4 minutes ago   Up 5 minutes   0.0.0.0:27017->27017/tcp   docker-testapp-mongo-1        
PS E:\Docker\docker-testapp>
```

Observe : default network being created, and no need of `version ` specification
- Rechek again if running at `http://localhost:8081/`
- Note , all previous data is gone, to retain persistance, we use `volumes`
- Try setting up the application by running `server.js` same as PART-2

<br>

## 6. You may delete the containers after use
  ```bash
  PS E:\Docker\docker-testapp> docker compose -f mongodb.yaml down
  time="2025-06-04T17:19:17+05:30" level=warning msg="E:\\Docker\\docker-testapp\\mongodb.yaml: the attribute `version` is obsolete, it will be ig
  nored, please remove it to avoid potential confusion"

  [+] Running 3/3
   ✔ Container docker-testapp-mongo-1          Removed                                                                                       3.0s 
   ✔ Container docker-testapp-mongo-express-1  Removed                                                                                       3.4s 
   ✔ Network docker-testapp_default            Removed
  ```
