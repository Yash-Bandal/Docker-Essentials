# WSL2 + Ubuntu + Docker + SSH Cheat Sheet

A quick-reference guide for managing the WSL2 Ubuntu environment, Docker containers, SSH access, and networking.

---

## 1. WSL2 — Windows PowerShell

**List installed Linux distributions**
```powershell
wsl --list --verbose
```

**Start Ubuntu**
```powershell
wsl -d Ubuntu-24.04
```

**Open default Ubuntu**
```powershell
wsl
```

**Shut down all WSL distributions**
```powershell
wsl --shutdown
```

**Check WSL version**
```powershell
wsl --version
```

**Open WSL configuration**
```powershell
notepad "$env:USERPROFILE\.wslconfig"
```

---

## 2. Ubuntu — Basic Linux Commands

**Check current user**
```bash
whoami
```

**Check current directory**
```bash
pwd
```

**List files and directories**
```bash
ls -la
```

**Navigate to a directory**
```bash
cd folder-name
```

**Go to home directory**
```bash
cd ~
```

**Update package information**
```bash
sudo apt update
```

**Upgrade installed packages**
```bash
sudo apt upgrade
```

**Install a package**
```bash
sudo apt install package-name
```

**Check disk space**
```bash
df -h
```

**Check memory usage**
```bash
free -h
```

**View running processes**
```bash
ps aux
```

---

## 3. systemd — Service Management

**Check systemd status**
```bash
systemctl status
```

**Check Docker service**
```bash
sudo systemctl status docker
```

**Check SSH service**
```bash
sudo systemctl status ssh
```

**Check SSH socket**
```bash
sudo systemctl status ssh.socket
```

**Start a service**
```bash
sudo systemctl start SERVICE
```

**Stop a service**
```bash
sudo systemctl stop SERVICE
```

**Restart a service**
```bash
sudo systemctl restart SERVICE
```

**Enable a service at startup**
```bash
sudo systemctl enable SERVICE
```

**Disable a service at startup**
```bash
sudo systemctl disable SERVICE
```

**Reload systemd configuration**
```bash
sudo systemctl daemon-reload
```

---

## 4. Docker — Container Management

**Check Docker version**
```bash
docker --version
```

**Check Docker Compose version**
```bash
docker compose version
```

**Check Docker service**
```bash
sudo systemctl status docker
```

**Start Docker**
```bash
sudo systemctl start docker
```

**Stop Docker**
```bash
sudo systemctl stop docker
```

**Restart Docker**
```bash
sudo systemctl restart docker
```

**Enable Docker at startup**
```bash
sudo systemctl enable docker
```

**List running containers**
```bash
docker ps
```

**List all containers**
```bash
docker ps -a
```

**List downloaded images**
```bash
docker images
```

**View container logs**
```bash
docker logs CONTAINER_NAME
```

**Follow container logs**
```bash
docker logs -f CONTAINER_NAME
```

**Inspect a container**
```bash
docker inspect CONTAINER_NAME
```

**Monitor live resource usage**
```bash
docker stats
```

**Remove stopped containers**
```bash
docker container prune
```

**Remove unused images**
```bash
docker image prune
```

**Run a test container**
```bash
docker run hello-world
```

---

## 5. Docker Compose — Application Management

Run these commands from the directory containing your `compose.yaml` or `docker-compose.yml` file.

**Start services in background**
```bash
docker compose up -d
```

**Build and start services**
```bash
docker compose up -d --build
```

**Stop and remove services**
```bash
docker compose down
```

**View running services**
```bash
docker compose ps
```

**View service logs**
```bash
docker compose logs
```

**Follow service logs**
```bash
docker compose logs -f
```

**Restart services**
```bash
docker compose restart
```

**Rebuild services**
```bash
docker compose build
```

---

## 6. SSH — Server Management

**Check SSH service**
```bash
sudo systemctl status ssh
```

**Start SSH**
```bash
sudo systemctl start ssh
```

**Restart SSH**
```bash
sudo systemctl restart ssh
```

**Check SSH socket**
```bash
sudo systemctl status ssh.socket
```

**Check listening ports**
```bash
sudo ss -ltnp
```

**Check port 2222**
```bash
sudo ss -ltnp | grep 2222
```

**Connect to local Ubuntu using SSH**
```bash
ssh -p 2222 -i ~/.ssh/id_ed25519 deltax@127.0.0.1
```

**View public SSH key**
```bash
cat ~/.ssh/id_ed25519.pub
```

**View authorized keys**
```bash
cat ~/.ssh/authorized_keys
```

**Generate an ED25519 SSH key**
```bash
ssh-keygen -t ed25519 -C "kamal-wsl-deployment"
```

**Validate SSH configuration**
```bash
sudo sshd -t
```

**Exit SSH session**
```bash
exit
```

---

## 7. Networking — Diagnostics

**View listening TCP ports**
```bash
sudo ss -ltnp
```

**Check a specific port**
```bash
sudo ss -ltnp | grep PORT
```

**Test HTTP endpoint**
```bash
curl http://localhost:PORT
```

**Test HTTPS endpoint (ignore certificate verification)**
```bash
curl -k https://localhost:PORT
```

**Check IP addresses**
```bash
ip addr
```

**Check network routes**
```bash
ip route
```

---

## 8. SSH — File Permissions

**Create SSH directory**
```bash
mkdir -p ~/.ssh
```

**Set SSH directory permissions**
```bash
chmod 700 ~/.ssh
```

**Set private key permissions**
```bash
chmod 600 ~/.ssh/id_ed25519
```

**Set public key permissions**
```bash
chmod 644 ~/.ssh/id_ed25519.pub
```

**Set authorized_keys permissions**
```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## 9. Quick Verification

Use these commands whenever you want to verify that your environment is working.

### WSL — Windows PowerShell

```powershell
wsl --list --verbose
```

### Ubuntu

```bash
whoami
```

```bash
systemctl status
```

### Docker

```bash
docker --version
```

```bash
docker ps
```

```bash
sudo systemctl status docker
```

### SSH

```bash
sudo systemctl status ssh
```

```bash
sudo ss -ltnp | grep 2222
```

### SSH Connection Test

```bash
ssh -p 2222 -i ~/.ssh/id_ed25519 deltax@127.0.0.1
```

---

## 10. Important Notes

- **PowerShell commands:** Execute these in Windows PowerShell.
- **Ubuntu commands:** Execute these inside your WSL Ubuntu terminal.
- **Docker commands:** Execute inside Ubuntu, preferably from your project directory when using Docker Compose.
- **SSH commands:** Execute inside Ubuntu unless explicitly specified otherwise.
- Replace placeholders such as `SERVICE`, `PORT`, `CONTAINER_NAME`, and `folder-name` with actual values.
- Commands like `docker container prune` and `docker image prune` remove unused resources. Review before confirming.
