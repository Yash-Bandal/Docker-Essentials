<h1 align = "center"> <b> 14 Docker Networking  </b></h1>

## 1. What is Docker Networking?

Docker networking allows containers to communicate with each other, the host system, and the outside world. Each container has its own network stack, including:

* IP address
* Ports
* Routing
* DNS

<br>

---

<br>

## 2. Why Use Docker Networks?

* Isolate containers in private networks
* Enable inter-container communication
* Control external and internal access
* Support multi-host networks in Docker Swarm

<br>

---

<br>

## 3. Types of Docker Networks

### 3.1 bridge (default)
* Created automatically
* Containers on the same bridge can communicate
* Each container gets its own IP address
* Useful for isolated environments <br>
  Two Types :<br>
    a. Default <br>
    b. custom (our nodejs app bridge network connecting mongo and mongo-express)

### 3.2 host

* Container created without IP address
* Container shares the host's network stack
* No isolation between host and container
* Linux-only feature

### 3.3 none

* Networking disabled for the container
* Used for complete isolation or testing

### 3.4 overlay

* Used in Docker Swarm
* Enables multi-host communication

### 3.5 macvlan

* Assigns MAC addresses to containers
* Appears as physical devices on the LAN
* Useful for direct network access

<br>

---

<br>

## 4. Common Commands

### 4.1 List Networks

```bash
docker network ls
```

### 4.2 Create Custom Bridge Network

```bash
docker network create my-network
```

### 4.3 Run Containers in Custom Network

```bash
docker run -it --name app1 --network my-network ubuntu
docker run -it --name app2 --network my-network ubuntu
```

Containers can now communicate using container names:

```bash
ping app2
```

### 4.4 Inspect Network

```bash
docker network inspect my-network
```

### 4.5 Remove Network

```bash
docker network rm my-network
```

<br>

---

<br>

## 5. Docker Compose and Networking

* Docker Compose automatically creates a private bridge network
* Services in the same file can communicate using service names

Example:

```yaml
services:
  backend:
    image: my-backend
  frontend:
    image: my-frontend
    depends_on:
      - backend
```

`frontend` can reach `backend` via `backend` as hostname.


| Feature             | bridge | host | none | overlay | macvlan |
| ------------------- | ------ | ---- | ---- | ------- | ------- |
| Default             | Yes    | No   | No   | No      | No      |
| Isolated IP         | Yes    | No   | Yes  | Yes     | Yes     |
| Inter-container DNS | Yes\*  | No   | No   | Yes     | No      |
| Cross-host Support  | No     | No   | No   | Yes     | No      |
| Compose Compatible  | Yes    | No   | No   | Yes     | No      |


<br>

---

<br>
