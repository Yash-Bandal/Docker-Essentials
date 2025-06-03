# 8 Using Containers to build Node Application

## Dockerizing a Real Application: Node.js + MongoDB
Let’s now take everything we’ve learned and apply it to a real-world project. <br>
We’ll build and run a Node.js + MongoDB application using Docker.

### Application Overview
We already have a basic [Node.js application](https://github.com/shradha-khapra/docker-testapp) set up. <br>
Here's what it includes:

    1. A `server.js` file — the backend logic using Express and MongoDB client
    2. Two API routes:
        2.1 GET /users — fetches user data
        2.2 POST /users — adds new user data
    3. A simple frontend (index.html, navigator.html) located in a public/ folder
    4. The server listens on port 5050
    5. It connects to a MongoDB database named apna_college_db
Currently, we don’t have MongoDB installed locally — and we won’t install it either.

Instead, we’ll use Docker to: 
      - Run a MongoDB database container
      - Use another container called mongo-express — a browser-based UI to interact with the database

###  Docker Images to Use
```
> mongo: Official MongoDB image = Database <br>
> mongo-express: Web UI for MongoDB =  admin interface to connect with the mongodb database
```

Lets consider the  `Node.js` Application settup


 [Networking](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/Terms.md#4-networking-in-docker) <br>
 [Network Commands](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/Pre-requisites/03_Docker_Commands.md#5-docker-networks)

---
 > Quick Tip :  use [ `\`] or  [ `] [space symbol] at every CLI cmd end to allow cmd on next line without Enter
---



### Steps

1. **Create a custom network** (e.g., `mongo-network`)

2. No need to pull the Mongo image manually from Docker Hub — it will auto-pull when not found locally.

3. We Run the container directly.

4. Now we setup our required containers inside network which could not communicate with PORTs,

5. **Set up Mongo container**

   * Refer: [DockerHub - Mongo](https://hub.docker.com/_/mongo)

   * Key run specifications:

     * `-d` detached mode
     * `-p 27017:27017` — map port
     * `--name mongo`
     * `--network mongo-network` — to run inside our isolated custom network

   * Set up the Two main environment variables:

     ```
     MONGO_INITDB_ROOT_USERNAME=admin  
     MONGO_INITDB_ROOT_PASSWORD=qwerty
     ```

   * Full command:

     ```
     docker run `
     -d `
     -p27017:27017 `
     --name mongo `
     --network mongo-network `
     -e MONGO_INITDB_ROOT_USERNAME=admin `
     -e MONGO_INITDB_ROOT_PASSWORD=qwerty `
     ```
         
    Finally write IMAGE name (Auto pulls latest Mongo Image from hub)

   * Output (example):

     ```bash
      PS C:\Users\admin> docker run `
        > -d `
        > -p27017:27017 `
        > --name mongo `
        > --network mongo-network `
        > -e MONGO_INITDB_ROOT_USERNAME=admin `
        > -e MONGO_INITDB_ROOT_PASSWORD=qwerty `
        > mongo
       
      Unable to find image 'mongo:latest' locally
      latest: Pulling from library/mongo
      fe348f35d71c: Pull complete
      c414ac2a616f: Pull complete
      d9d352c11bbd: Pull complete
      44ff3887af58: Pull complete
      17e5496c463c: Pull complete
      2d10e96e8208: Pull complete
      886e2901cc3c: Pull complete
      370025c3b5a0: Pull complete
      Digest: sha256:48f35cea5297c6a041e4e1d09e7c2df278cbe96f1410d20ace5a0f911d0f48d0
      Status: Downloaded newer image for mongo:latest
      60954fa395d9f964efe5e0d7bf23cf35bcea49c34fb66abe626d3e9e862727af
       
      PS C:\Users\admin> docker ps -a
      CONTAINER ID   IMAGE   COMMAND                 CREATED                  STATUS           PORTS                     NAMES
      60954fa395d9   mongo   "docker-entrypoint.s…"  Less than a second ago   Up 40 seconds    0.0.0.0:27017->27017/tcp  mongo
     ```

6. Now **Run `mongo-express` container in the same network**

   * Required environment variables:

     ```bash
     ME_CONFIG_MONGODB_ADMINUSERNAME
     ME_CONFIG_MONGODB_ADMINPASSWORD
     ME_CONFIG_MONGODB_URL="mongodb://USERNAME:PASSWORD@mongo-container-name:27017"
     ```
     
   
   * Full command:

     ```bash
     docker run `
       -d `
       -p8081:8081 `
       --name mongo-express `
       --network mongo-network `
       -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin `
       -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty `
       -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" `
       mongo-express
     ```

   * Output (example):

     ```bash
     PS C:\Users\admin> docker run `
     >> -d `
     >> -p8081:8081 `
     >> --name mongo-express `
     >> --network mongo-network `
     >> -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin `
     >> -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty `
     >> -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" `                    
     >> mongo-express
     Unable to find image 'mongo-express:latest' locally
     latest: Pulling from library/mongo-express
     7e9a007eb24b: Pull complete
     0bf3571b6cd7: Pull complete
     5189255e31c8: Pull complete
     9f7f59574f7d: Pull complete
     45b24ec126f9: Pull complete
     619be1103602: Pull complete
     d8305ae32c95: Pull complete
     Digest: sha256:1b23d7976f0210dbec74045c209e52fbb26d29b2e873d6c6fa3d3f0ae32c2a64
     Status: Downloaded newer image for mongo-express:latest
     15f9c77888933e85f2f3c1cd7a69064328ba8968885b23583947ac3d4023fc20
     ```
    
     ```bash
     PS C:\Users\admin> docker ps -a
     CONTAINER ID   IMAGE           COMMAND                  CREATED                  STATUS                     PORTS
     NAMES
     094d594f633c   mongo           "docker-entrypoint.s…"   Less than a second ago   Up 43 seconds              0.0.0.0:27017->27017/tcp
     mongo
     15f9c7788893   mongo-express   "/sbin/tini -- /dock…"   21 minutes ago           Up 19 seconds              0.0.0.0:8081->8081/tcp
     ```

7. **Avoid mistaking the username `admin` and password `qwerty`**

   * While studying i mistaked the password and username of `mongo` container , if wrong credentials are used:

     * Either delete the container (not the image), or
     * Enter the bash of the `mongo` container:

       ```bash
       docker exec -it mongo bash
       root@container:/# mongosh -u WRONG_Username -p WRONGPASS --authenticationDatabase admin

       test> use admin
       admin> db.changeUserPassword("admin","qwerty")
       ```

8. Visit `http://localhost:8081/` in your browser

