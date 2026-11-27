# 01 – WSL2 & Ubuntu 24.04 Setup

## Objective
Set up Ubuntu 24.04 LTS on Windows 11 using WSL2 and enable systemd so Ubuntu can operate as our local Linux server.

## Requirements
- Windows 11
- Administrator access
- Internet connection
- Hardware virtualization enabled

## Step 1 – Install WSL2

**Terminal:** Windows PowerShell (Run as Administrator)

```powershell
wsl --install
```

If no Linux distribution gets installed, explicitly install Ubuntu:

```powershell
wsl --install -d Ubuntu-24.04
```

Restart Windows if prompted.

**Expected:** Ubuntu 24.04 downloads and installs successfully.

## Step 2 – Create Ubuntu user

Launch Ubuntu when installation completes.

Create a Unix username and password when prompted.

Example:

```text
Create a default Unix user account: deltax
New password:
Retype new password:
```

These credentials are independent of Windows credentials.

**Expected:** Ubuntu terminal opens with your Linux user.

## Step 3 – Verify Ubuntu installation

**Terminal:** Ubuntu

```bash
whoami
```

Expected:

```text
deltax
```

Check Ubuntu version:

```bash
lsb_release -a
```

Expected:

```text
Distributor ID: Ubuntu
Description: Ubuntu 24.04.5 LTS
Release: 24.04
Codename: noble
```

## Step 4 – Verify WSL2

**Terminal:** Windows PowerShell

```powershell
wsl --list --verbose
```

Expected:

```text
NAME            STATE      VERSION
Ubuntu-24.04    Running    2
```

The VERSION column must show `2`.

## Step 5 – Enable systemd

**Terminal:** Ubuntu

Open the WSL configuration file:

```bash
sudo nano /etc/wsl.conf
```

Add:

```ini
[boot]
systemd=true
```

Save with `Ctrl + O`, press Enter, and exit with `Ctrl + X`.

## Step 6 – Restart WSL

**Terminal:** Windows PowerShell

```powershell
wsl --shutdown
```

Launch Ubuntu again:

```powershell
wsl -d Ubuntu-24.04
```

## Step 7 – Verify systemd

**Terminal:** Ubuntu

```bash
systemctl status
```

Expected: systemd reports `State: running`.

Verify PID 1:

```bash
ps -p 1 -o comm=
```

Expected:

```text
systemd
```

## Completion Checklist

- [x] WSL2 installed
- [x] Ubuntu 24.04 LTS installed
- [x] Linux user created
- [x] WSL version verified as 2
- [x] systemd enabled
- [x] systemd verified as PID 1

## Result

Ubuntu 24.04 is running inside WSL2 with systemd enabled.

The environment is ready for installing Docker Engine and configuring Linux server services.

## Important Notes

- `wsl` commands must run in Windows PowerShell, not Ubuntu.
- `systemctl` commands must run inside Ubuntu.
- `wsl --shutdown` must be executed from Windows PowerShell.
- WSL1 compatibility warnings do not mean WSL2 has failed.
