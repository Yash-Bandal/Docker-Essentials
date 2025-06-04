<h1 align ="center"><b> 11  Publishing to DockerHub </b></h1>

[Reference](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)
<br>

#  A. Dockerizing our own Application
Test-app -> Image -> Container ->upload

- Here we are creating our own Docker container, but in general practical applications `CICD` pipelines with `Jenkins` are used. 
- This is now shared on public or private repo

# Dockerfile Instructions

## 1. Introduction

A **Dockerfile** is a script that contains a series of instructions to build a Docker image. Below are some of the most commonly used instructions.

<br>

## 2. Common Dockerfile Instructions

### 2.1 `FROM`

```Dockerfile
FROM <image>
```

* Specifies the **base image** for the build. eg for our `node` app, [base image](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/02_PART-2.md#1-layers-in-dockerimages) is `Node` installed
* Every Dockerfile must start with a `FROM` instruction.

**Example:**

```Dockerfile
FROM python:3.11-slim
```

<br>

### 2.2 `WORKDIR`

```Dockerfile
WORKDIR <path>
```

* Sets the **working directory** inside the container.
* All subsequent instructions (like `RUN`, `COPY`, `CMD`) will be executed in this directory.

**Example:**

```Dockerfile
WORKDIR /app
```

<br>

### 2.3 `COPY`

```Dockerfile
COPY <host-path> <image-path>
```

* Copies files/folders from the **host machine** into the container **image**.

**Example:**

```Dockerfile
COPY . /app
```

<br>

### 2.4 `RUN`

```Dockerfile
RUN <command>
```
We can have multiple `RUN` , but 1 `CMD` commands
* Executes commands inside the image **at build time**.
* Used for installing dependencies or building the app.

**Example:**

```Dockerfile
RUN pip install -r requirements.txt
```

<br>

### 2.5 `ENV`

```Dockerfile
ENV <name> <value>
```

* Sets an **environment variable** inside the container.

**Example:**

```Dockerfile
ENV NODE_ENV=production
```

<br>

### 2.6 `EXPOSE`

```Dockerfile
EXPOSE <port-number>
```

* Documents which port the container will **listen on** at runtime.
* This does **not** publish the port—use `-p` in `docker run`.

**Example:**

```Dockerfile
EXPOSE 3000
```

<br>

### 2.7 `USER`

```Dockerfile
USER <user-or-uid>
```

* Sets the **default user** for the remaining instructions.

**Example:**

```Dockerfile
USER appuser
```

<br>

### 2.8 `CMD`

```Dockerfile
CMD ["<command>", "<arg1>", ...]
```

* Sets the **default command** to run when the container starts.
* Only one `CMD` is allowed; later ones override earlier ones.

---
> Imagine Once using `Run` our full container is settup, we have to choose that `one` command to execute to Run the application, this is `CMD`
---

**Example:**

```Dockerfile
CMD ["python", "app.py"]
```

---

<br>


| Instruction | Purpose                                          |
| ----------- | ------------------------------------------------ |
| `FROM`      | Defines the base image                           |
| `WORKDIR`   | Sets working directory inside container          |
| `COPY`      | Copies files from host to image                  |
| `RUN`       | Runs a command during image build                |
| `ENV`       | Sets environment variable                        |
| `EXPOSE`    | Indicates container's listening port             |
| `USER`      | Sets the user for following instructions         |
| `CMD`       | Specifies the default command on container start |


<br>

### 1. Create a Dockerfile named exactly **`Dockerfile`** in our `test-app`
 - After creating file, first set the base image, here `node` is the base image with its above [layer](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/02_PART-2.md#1-layers-in-dockerimages) as our `testapp`
 - Note that , the `node` base layer will have its own **base layers** which is [debian:bookworm-slim](https://github.com/nodejs/docker-node/blob/main/24/bookworm-slim/Dockerfile)

**12 Layering :**

---
> Debian base layer-> Node base layer -> Our test-app -> Over it , there can be another image we create later
---

<br>

We copy the files and contents of out app inside another file `testapp` which is also the image name

**Architecture**
```

Root docker-testapp
    |--Test-app
        |-copy files and packages from host 1
        |-copy  2
        |-copy  3
        |-copy  4


```

<br>

Dockerfile
```Dockerfile
FROM node

# one line or \
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty

# Make a test-app directory inside root app
RUN mkdir -p testapp

#if there were no node modules files
# RUN npm install

#operation : COPY <host-path of  docker-test-app>  <image-path testapp>
COPY . /testapp

#set working directory (vimp)
WORKDIR /testapp

#commands as array of strings
#note that the server.js is inside /testapp not root
#cmd `node /testapp/server.js`
CMD ["node", "/testapp/server.js"]
```
can set Port inside Dockerfile
```Dockerfile
ENV PORT=5050
```

<br>

### 2. Run the `build` command to start IMAGE creation
```bash
docker build -t <IMAGE-NAME>:1.0 (version you want)
```
```bash
PS E:\Docker\docker-testapp> docker build -t testapp:1.0 .
[+] Building 373.1s (9/9) FINISHED
```

```bash
PS E:\Docker\docker-testapp> docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
testapp         1.0       a16c0aa53e85   58 seconds ago   1.64GB
```
If anything you add later, no need to delete the IMAGE completely, <br>
Just rebuild using above command

<br>

### 3. Run the container
a. Non interactive mode (my case..didnt worked..may be due no WORKDIR set , which I set later..and thenit worked)
```bash
PS E:\Docker\docker-testapp> docker run -p 5050:5050 testapp:1.0
server running on port 5050
```
b. Interactive mode (worked effectively)
```bash
PS E:\Docker\docker-testapp> docker run -it -p 5050:5050 testapp:1.0 bash

root@7b86ce4a1846:/testapp# ls
Dockerfile  README.md  mongodb.yaml  node_modules  package-lock.json  package.json  public  server.js
```
If no Default node modules..RUN npm install inside dockerfile
```
root@7b86ce4a1846:/testapp# node server.js
server running on port 5050
```

Both working Perfectly, just done forget Port Binding <br>
Now you can try using Docker compose to Run all the three containers, at a time
- the mongo, mongo-express and testapp build container

<br>

---

<br>

#  B. Pushing on Docker Hub
### 1. Create a repository within docker hub
- You will see username/repo name after created
- eg. `yashbandal25/testapp`

<br>

### 2. Recreate Image with name yashbandal25/testapp (takes 2 - 3 seconds)
```bash
PS E:\Docker> cd .\docker-testapp\

PS E:\Docker\docker-testapp> docker build -t yashbandal25/testapp .
[+] Building 3.5s (10/10) FINISHED                    docker:desktop-linux


PS E:\Docker\docker-testapp> docker images
REPOSITORY             TAG       IMAGE ID       CREATED          SIZE
yashbandal25/testapp   latest    b5dbbe49d227   48 minutes ago   1.64GB

```

<br>

### 3. Login the terminal with same DockerHub account
- logout if not logged in previosly with same account
  ```bash
  PS E:\Docker\docker-testapp> docker logout
  
  Removing login credentials for https://index.docker.io/v1/
  2025/06/04 20:37:09 notifying Desktop of credentials store update: getting credential helper update: no token found in credential manager for 'i
  ndex.docker.io'

  ```

- Login using 2 ways
  ```bash
  docker login
  ```
  and enter Code in brower, or use below command
  ```bash
  i Info → To sign in with credentials on the command line, use 'docker login -u <username>'
  
  
  Your one-time device confirmation code is: XXXX-XXXX
  Press ENTER to open your browser or submit your device code here: https://login.docker.com/activate
  
  Waiting for authentication in the browser…
  Login Succeeded
  ```


<br>

### 4. Push  Repository to Docker Hub
**Command :**
```bash
docker push username/reponame
```

<br>

```bash
PS E:\Docker\docker-testapp> docker push yashbandal25/testapp
Using default tag: latest
The push refers to repository [docker.io/yashbandal25/testapp]
```
