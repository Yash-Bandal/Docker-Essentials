<h1 align="center"><b>13 Volume Mounting</b></h1>

# A. Docker Volumes 
Docker volumes are the mechanisms that provide a way to store and persist data for containers outside of the container's writable layer, ensuring data remains even when a container is stopped or deleted. 
- By default, if we stop a running container, all data is lost and we have a fresh start ..so to persist we use Volumes.
- Docker sets aside a separate space on your host system (managed by Docker). for this volume
- Whatever data we write inside the container's target folder is now also saved in the volume.
- We mount this space with the container,
- Now, Even if the container is deleted, the data remains in the volume.
We generally run in Interactive mode, so that we can see what processes ar happening within

  ---
  > We can mount a single Volume space with 1+contauners also
  ---

<br>

<table>
  <tr>
    <td>
      <img src="https://github.com/Yash-Bandal/Docker-Essentials/blob/0780724699245b691004a4907d913be215b24b8e/Notes/StoreRoom/13DockerVoll.png" width="500">
      <h3>Multiple Containers in a Volume</h3>
    </td>
  <td>
      <img src="https://github.com/Yash-Bandal/Docker-Essentials/blob/0780724699245b691004a4907d913be215b24b8e/Notes/StoreRoom/13_2.png" width="500">
      <h3> Volume Bind to Container and Host </h3>
    </td>
  </tr>
</table>


<br>

**Steps :**
### 1. Create a volume using `-v` from test-app location / any
```bash
-v host_path(/absolute/path) : container_path
```
**Command :**
Here `data` is our Volume directory that is auto created on `Desktop`
```bash
PS E:\Docker\docker-testapp> docker run -it -v /c/Users/admin/Desktop/data:/test/data ubuntu
root@85a84891b1cc:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
```
Above, we see `test\data` directory is created inside ubuntu IMAGE..

---
> Note : use correct absolute path ..<br>
> Windows : /c/Users/admin/Desktop/data <br>
> Ubuntu IMAGE : /test/data
---

<br>

```bash
root@85a84891b1cc:/# cd test
root@85a84891b1cc:/test# ls
data
```
Now we see a `data` folder for our volume is, also auto created on Desktop

Now create two files inside ubuntu IMAGE <br>
You see that automatically, the same files are created inside Desktop/data <br>
This marks the `Binding` of Volume directory inside Ubuntu Image and Windows, ..that is a `Shared Memory`

```bash
root@85a84891b1cc:/test# cd data
root@85a84891b1cc:/test/data# ls
root@85a84891b1cc:/test/data# touch index.html
root@85a84891b1cc:/test/data# touch server.js 
root@85a84891b1cc:/test/data# ls
index.html  server.js
```
`index.html`,  `server.js` are also present on Desktop data directory.

<br>

### 2. Now try exiting and stopping the container

<br>

### 3. You see, that `index.html`,  `server.js` are not lost
- Generally without Volumes, the data would have lost within image, but using volumes `-v` a redundant copy of files is created on binded Host os (/Desktop/data)

<br>

### 4. Verify `persistancy` inside image
- stop container `Ubuntu` and restart it
  ```bash
  root@0872154f290b:/test/data# exit
  exit

  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```
  Rechek files if present: 
  ```bash
  PS E:\Docker\docker-testapp> docker exec -it 85a84891b1ccc10659c41fcdb0a5dfa7ab55dd9aa0eb61036cb2614c72a09b69 bash
  
  root@85a84891b1cc:/# ls
  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
  
  root@85a84891b1cc:/# cd test
  root@85a84891b1cc:/test# ls
  data
  
  root@85a84891b1cc:/test# cd data
  root@85a84891b1cc:/test/data# ls
  index.html  server.js
  ```
  Thus, the image persists the files even after rerun.

  Even if you delete container, data persists in the Desktop directory

  <br>

  ---

  <br>


# B. Using Docker Volumes with Docker Compose (MongoDB Example)
Earlier, when we created a database (e.g., college) and added documents inside MongoDB (via mongo-express), they disappeared once we:
- Stopped the container
- Or deleted it
This is because Docker containers, by default, do not persist data.

To ensure persistance, we use Docker Compse `YAML`

### Step 1: Modify the docker-compose.yml
- Add a volume under the MongoDB service
```yaml
services :
  mongo :
     .
     .
    volumes :
        - host_direcoty_path : container_directory_path (Mongo ..not UBUNTU) (/data/db)
```


<br>

Updated `mongodb.yaml`
```yaml
version : "3.8"

services :  #containers and its requirements
  mongo : 
    image : mongo 
    ports : 
    - 27017:27017
    environment :
      MONGO_INITDB_ROOT_USERNAME : admin  
      MONGO_INITDB_ROOT_PASSWORD : qwerty

    #single change
    volumes :
    - /c/Users/admin/Desktop/data:/data/db

  mongo-express :
    image : mongo-express
    ports :
      - 8081:8081
    environment :
      ME_CONFIG_MONGODB_ADMINUSERNAME : admin
      ME_CONFIG_MONGODB_ADMINPASSWORD : qwerty 
      ME_CONFIG_MONGODB_URL : mongodb://admin:qwerty@mongo:27017/ # remove "" and add / at end 

```
  
<br>

### Step 2: Run Docker Compose `up`
```bash
docker compose -f mongodb.yaml up
```

Terminal:
```bash
PS E:\Docker\docker-testapp> docker compose -f mongodb.yaml up
time="2025-06-04T23:07:35+05:30" level=warning msg="E:\\Docker\\docker-testapp\\mongodb.yaml: the attribute `version` is ob
solete, it will be ignored, please remove it to avoid potential confusion"
[+] Running 2/2
 ✔ Container docker-testapp-mongo-express-1  Created                                                                 
 ✔ Container docker-testapp-mongo-1          Created
```
Prints a long message....

Now: <br>
Mongo and Mongo Express will start
- The volume will be mounted automatically

If you now:
- Create a DB (e.g., yash-db)
- Add a collection (e.g., users)
- Insert a document {}

The data will be stored in the mounted volume (Desktop/data). (Redundancy withing mongo image and Desktop host device)

<img src = "https://github.com/Yash-Bandal/Docker-Essentials/blob/0780724699245b691004a4907d913be215b24b8e/Notes/StoreRoom/13_Compose.PNG" width=500>

We see that , using compose we created 2 containers(`mongo` , `mongo-express`) inside our `testapp` container within 1 volume mounted to `/Desktop/data`<br>

<br>

- Click on the `mongo-express` 27017 port -> Enter uname , pass `admin`, `pass` and create database `yash-db`,
- Within `yash-db` create a Document and save

  ## Verification
  ###  Step 3: Stop and Remove All Containers using `down`

  ```bash
  PS E:\Docker\docker-testapp> docker compose -f mongodb.yaml down
  time="2025-06-04T23:25:55+05:30" level=warning msg="E:\\Docker\\docker-testapp\\mongodb.yaml: the attribute `version` is ob
  solete, it will be ignored, please remove it to avoid potential confusion"
  [+] Running 3/3
   ✔ Container docker-testapp-mongo-express-1  Removed                                                                 
   ✔ Container docker-testapp-mongo-1          Removed                                                                  
   ✔ Network docker-testapp_default            Removed     
   ```

  ###  Step 4: Run Docker Compose Again `up`
  ```bash
  PS E:\Docker\docker-testapp> docker compose -f mongodb.yaml up
  
  time="2025-06-04T23:28:09+05:30" level=warning msg="E:\\Docker\\docker-testapp\\mongodb.yaml: the attribute `version` is ob
  solete, it will be ignored, please remove it to avoid potential confusion"
  [+] Running 3/3
   ✔ Network docker-testapp_default            Created                                                               
   ✔ Container docker-testapp-mongo-1          Created                                                                 
   ✔ Container docker-testapp-mongo-express-1  Created   
    ```

  <img src = "https://github.com/Yash-Bandal/Docker-Essentials/blob/e24f6c945027edfdef2d255afdbfd27a8ee59bc0/Notes/StoreRoom/13_persistance.PNG" width = 500>

  Thus we observe, all our data persists, this is all because of Volumes

  ---

  ## Docker Volume Commands and Types
  [Volume Commands](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/Pre-requisites/03_Docker_Commands.md#4-docker-volumes)


  ---
