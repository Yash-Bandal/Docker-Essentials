# Lesson: WSL (Windows Subsystem for Linux) Commands

This lesson covers the most commonly used WSL commands for managing Linux distros on Windows, including Docker-specific use cases.

<br>

### What is Distro?

In the context of Windows Subsystem for Linux (WSL), a "distro" refers to **a specific Linux distribution that you install and run within your Windows environment**. WSL allows you to access and use various Linux distributions like Ubuntu, Debian, or Kali, without needing a virtual machine or dual booting

<br>

## 1. Basic WSL Commands

### A. Launch Default Distro

```bash
wsl
```

**Output:** Opens the default Linux shell in the terminal.

<br>

### B. List Installed Distros

```bash
wsl --list
```

**Output:**

```
Windows Subsystem for Linux Distributions:
Ubuntu (Default)
Debian
kali-linux
```

<br>

### C.  List Distros with Details 🚩

```bash
wsl --list --verbose
wsl -l -v
```

**Output:**

```
NAME            STATE           VERSION
* Ubuntu        Running         2
  Debian        Stopped         1
  kali-linux    Stopped         2
```


<br>

### D. Set WSL Version for a Distro

```bash
wsl --set-version Ubuntu 2
```

**Output:** Converts Ubuntu to use WSL 2.

<br>

### E. Set Default WSL Version

```bash
wsl --set-default-version 2
```

**Output:** Future distros will use WSL 2.

<br>
<br>

## 2. Running Commands Inside WSL

### Run a Single Command

```bash
wsl ls ~
```

**Output:** Lists contents of home directory in default distro.

<br>

### Run Command in Specific Distro

```bash
wsl -d Debian uname -a
```

**Output:** Shows Linux system information for Debian distro.

<br>
<br>

## 3.  Distro Management

### Install WSL with Default Distro

```powershell
wsl --install
```

**Output:** Installs WSL and default Ubuntu distro.

<br>

### Unregister (Delete) a Distro

```bash
wsl --unregister Debian
```

**Output:** Removes Debian distro.

<br>

### Export a Distro

```bash
wsl --export Ubuntu ubuntu_backup.tar
```

**Output:** Creates a TAR backup of Ubuntu.

### Import a Distro

```bash
wsl --import NewUbuntu D:\WSL\NewUbuntu ubuntu_backup.tar
```

**Output:** Installs NewUbuntu distro from backup TAR file.

<br>
<br>

## 4.  Configuration and Updates

### Check WSL Status

```bash
wsl --status
```

**Output:**

```
Default Version: 2
WSL is installed and the default distro is Ubuntu.
```
<br>

### Shutdown All WSL Instances

```bash
wsl --shutdown
```

**Output:** All running WSL distros are terminated.

<br>

### Update WSL Kernel

```bash
wsl --update
```

**Output:** Downloads and installs latest WSL kernel.


<br>
<br>

## Finally close wsl 

```
exit
```

<br>
<br>

## 5. 🐳 Docker-Specific WSL Usage

### Enable Docker to Use WSL 2 Backend

Ensure Docker Desktop is configured to use WSL 2:

* Go to Docker Desktop → Settings → General
* Check "Use the WSL 2 based engine"

### Set Docker to Use a Specific Distro
  * Go to Docker Desktop → Settings → Resources → WSL Integration
  * Enable integration for your desired distro (e.g., Ubuntu)

### Verify Docker is Running in WSL

```bash
wsl -d Ubuntu docker info
```

**Output:** Docker system info, verifying it's running in WSL.

### Run Docker Command in WSL

```bash
wsl -d Ubuntu docker run hello-world
```

**Output:**

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

<br>

##  Summary Table

| Command                            | Description                    |
| ---------------------------------- | ------------------------------ |
| `wsl`                              | Open default Linux shell       |
| `wsl --list`                       | List installed distros         |
| `wsl -l -v`                        | List distros with version info |
| `wsl --set-version <Distro> <Ver>` | Convert to WSL version 1 or 2  |
| `wsl --shutdown`                   | Shutdown all WSL distros       |
| `wsl --export` / `--import`        | Backup and restore distros     |
| `wsl -d <Distro>`                  | Run command in specific distro |
| `wsl --install`                    | Install WSL and default distro |


---
> Use WSL to bridge the power of Linux tools and Docker with the Windows environment efficiently.
---

-> Edit and turn of Con

<br>
<br>

---
## Errors running Docker?
- Docker engine runs in background...once yo close start its dashboard from Desktop taskbar icon , near wifi options
- If not working Engine
   - Check windows security -> App and broswer control -> explot protection -> Program settings -> find scroll down C::\Windows\...uncompute.exe -> edit and trun of CFG(control flow guard)
   - search in settings `Turn features on or off` and turn windows powershell and other requirements
   - using task manager end tasks after shutdown wsl -shutdown
