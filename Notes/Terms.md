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


