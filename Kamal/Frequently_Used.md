# **WSL2 + Ubuntu + Docker + SSH Cheat Sheet**

## **1. WSL2 — Windows PowerShell**

```
# List installed Linux distributions
wsl --list --verbose

# Start Ubuntu
wsl -d Ubuntu-24.04

# Open default Ubuntu
wsl

# Shut down all WSL distributions
wsl --shutdown

# Check WSL version
wsl --version

# Open WSL configuration
notepad "$env:USERPROFILE\.wslconfig"
```

## **2. Ubuntu — Basic Linux Commands**

```
# Current user
whoami

# Current directory
pwd

# List files
ls -la

# Navigate directory
cd folder-name

# Go to home directory
cd ~

# Update package information
sudo apt update

# Upgrade installed packages
sudo apt upgrade

# Install package
sudo apt install package-name

# Check disk space
df -h

# Check memory
free -h

# Running processes
ps aux
```

## **3. systemd — Service Management**

```
# Check systemd status
systemctl status

# Check Docker service
sudo systemctl status docker

# Check SSH service
sudo systemctl status ssh

# Check SSH socket
sudo systemctl status ssh.socket

# Start service
sudo systemctl start SERVICE

# Stop service
sudo systemctl stop SERVICE

# Restart service
sudo systemctl restart SERVICE

# Enable service at startup
sudo systemctl enable SERVICE

# Disable service at startup
sudo systemctl disable SERVICE

# Reload systemd configuration
sudo systemctl daemon-reload
```

## **4. Docker — Container Management**

```
# Check Docker version
docker --version

# Check Docker Compose version
docker compose version

# Check Docker service
sudo systemctl status docker

# Start Docker
sudo systemctl start docker

# Stop Docker
sudo systemctl stop docker

# Restart Docker
sudo systemctl restart docker

# Enable Docker at startup
sudo systemctl enable docker

# List running containers
docker ps

# List all containers
docker ps -a

# List downloaded images
docker images

# View container logs
docker logs CONTAINER_NAME

# Follow container logs
docker logs -f CONTAINER_NAME

# Inspect container
docker inspect CONTAINER_NAME

# Live resource usage
docker stats

# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Run test container
docker run hello-world
```

## **5. Docker Compose — Application Management**

```
# Start services
docker compose up -d

# Build and start services
docker compose up -d --build

# Stop services
docker compose down

# View running services
docker compose ps

# View logs
docker compose logs

# Follow logs
docker compose logs -f

# Restart services
docker compose restart

# Rebuild services
docker compose build
```

## **6. SSH — Server Management**

```
# Check SSH service
sudo systemctl status ssh

# Start SSH
sudo systemctl start ssh

# Restart SSH
sudo systemctl restart ssh

# Check SSH socket
sudo systemctl status ssh.socket

# Check listening ports
sudo ss -ltnp

# Check port 2222
sudo ss -ltnp | grep 2222

# Connect to local Ubuntu using SSH
ssh -p 2222 -i ~/.ssh/id_ed25519 deltax@127.0.0.1

# View public key
cat ~/.ssh/id_ed25519.pub

# View authorized keys
cat ~/.ssh/authorized_keys

# Generate SSH key
ssh-keygen -t ed25519 -C "kamal-wsl-deployment"

# Validate SSH configuration
sudo sshd -t

# Exit SSH session
exit
```

## **7. Networking — Diagnostics**

```
# View listening TCP ports
sudo ss -ltnp

# Check a specific port
sudo ss -ltnp | grep PORT

# Test HTTP endpoint
curl http://localhost:PORT

# Test HTTPS endpoint
curl -k https://localhost:PORT

# Check IP addresses
ip addr

# Check routes
ip route
```

## **8. Useful File Permissions**

```
# Create SSH directory
mkdir -p ~/.ssh

# Set SSH directory permissions
chmod 700 ~/.ssh

# Set private key permissions
chmod 600 ~/.ssh/id_ed25519

# Set public key permissions
chmod 644 ~/.ssh/id_ed25519.pub

# Set authorized_keys permissions
chmod 600 ~/.ssh/authorized_keys
```

## **9. Quick Verification**

```
# WSL (PowerShell)
wsl --list --verbose

# Ubuntu
whoami
systemctl status

# Docker
docker --version
docker ps
sudo systemctl status docker

# SSH
sudo systemctl status ssh
sudo ss -ltnp | grep 2222

# SSH connection test
ssh -p 2222 -i ~/.ssh/id_ed25519 deltax@127.0.0.1
```
