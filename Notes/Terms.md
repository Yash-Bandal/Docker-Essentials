## 1. Docker Daemon

- The Docker Daemon (also known as dockerd) is the core background service that runs on your system and does all the actual heavy lifting in Docker.

- The Docker daemon is like a chef in a kitchen.
You (the user) place orders by giving commands, but the chef (daemon) does the cooking behind the scenes and makes/creates food (container)


<br>

## 2. Docker Hub
**Docker Hub** is a **cloud-based registry service** where you can:
1.  **Store** Docker images
2.  **Share** them with others
3.  **Download (pull)** images built by others    
4.  **Build and automate** your own image pipelines

Think of it like **GitHub, but for multipe Docker images**.

Like we `git clone ` repositories from github, we `pull` images from Docker Hub
- Example :- Try searching Ubuntu, helloworld image on hub

<br>

## 3. Docker Image Layers
- In Docker, **Docker Image layers** are the building blocks of Docker images. Every image is made up of a **stack of read-only layers**, where each layer represents an **instruction** in your `Dockerfile`.
-  A **layer** in Docker is a cached snapshot of a file system change — like adding files, installing software, or setting up environment variables.

<br>


## 4 Networking in Docker

By default, Docker containers are **isolated from each other**. Even though they may run on the same host, they don’t automatically see each other’s ports.
To allow multiple containers (like Node.js and MongoDB) to communicate:
-   We create a **custom Docker network**

This network allows:
-   The Node.js container to talk to MongoDB using the container’s **name** as the hostname    
-   mongo-express to access the MongoDB container securely

 [Network Commands](https://github.com/Yash-Bandal/Docker-Essentials/blob/main/Notes/Pre-requisites/03_Docker_Commands.md#5-docker-networks)


