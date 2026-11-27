# 02 – Docker Engine Installation on WSL2

## Objective

Install Docker Engine directly inside Ubuntu 24.04 running on WSL2.

We use Docker Engine rather than Docker Desktop because WSL2 will act as our Linux deployment server for Kamal.

## Requirements

- Windows 11
- WSL2 installed
- Ubuntu 24.04 LTS
- systemd enabled
- Internet connection

## Step 1 – Update package index

**Terminal:** Ubuntu

```bash
sudo apt update
```

**Expected:** Package lists update successfully without errors.

## Step 2 – Install prerequisites

```bash
sudo apt install ca-certificates curl gnupg
```

| Package | Purpose |
|---|---|
| ca-certificates | Verify HTTPS certificates |
| curl | Download files |
| gnupg | Cryptographic signature verification |

## Step 3 – Create Docker keyring directory

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Download Docker's official GPG key:

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

Set permissions:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

**Expected:** Docker's signing key is available at `/etc/apt/keyrings/docker.asc`.

## Step 4 – Add Docker's official repository

Execute:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Refresh package lists:

```bash
sudo apt update
```

**Expected:** Docker's repository appears in the package update output.

Example:

```text
https://download.docker.com/linux/ubuntu noble/stable amd64 Packages
```

## Step 5 – Install Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Confirm installation when prompted.

### Installed components

| Component | Responsibility |
|---|---|
| docker-ce | Docker Engine daemon |
| docker-ce-cli | Docker command-line interface |
| containerd.io | Container lifecycle management |
| docker-buildx-plugin | Advanced image building |
| docker-compose-plugin | Multi-container orchestration |

## Step 6 – Verify Docker installation

Check Docker version:

```bash
docker --version
```

Check Docker service:

```bash
sudo systemctl status docker
```

Expected:

```text
Active: active (running)
```

Test Docker:

```bash
sudo docker run hello-world
```

Expected:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

This confirms that the Docker client can communicate with the Docker daemon, pull an image and run a container.

## Step 7 – Configure Docker permissions

By default, Docker commands require `sudo`.

Add your Linux user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

Apply the updated group membership in the current terminal:

```bash
newgrp docker
```

Verify group membership:

```bash
groups
```

Expected output includes:

```text
docker adm cdrom sudo dip plugdev users deltax
```

Test Docker without sudo:

```bash
docker ps
```

Expected: Container list appears without a permission error.

## Completion Checklist

- [x] Docker prerequisites installed
- [x] Docker GPG key configured
- [x] Official Docker repository added
- [x] Docker Engine installed
- [x] Docker service running under systemd
- [x] `hello-world` container executed successfully
- [x] Linux user added to Docker group
- [x] Docker commands work without sudo

## Result

Docker Engine is installed and operational inside Ubuntu 24.04 on WSL2.

The environment is ready for configuring OpenSSH Server, which Kamal will use to deploy containers.

## Security Note

Membership in the `docker` group grants privileges equivalent to root access on the Linux machine. Only trusted users should be added to this group.
