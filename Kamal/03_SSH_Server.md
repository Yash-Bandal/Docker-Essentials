# 03 – OpenSSH Server & Key-Based Authentication on WSL2

## Objective

Configure Ubuntu 24.04 running inside WSL2 as an SSH-accessible Linux server.

Kamal will use SSH to connect to this machine and execute deployment operations, even though the client and server run on the same physical laptop.

### Need of SSH Server
<img width="557" height="181" alt="image" src="https://github.com/user-attachments/assets/94aeb01a-fffb-4976-9585-8357da6688fd" />

```
sudo systemctl status ssh
```

| Command |	Purpose |
|-- | -- |
|enable |	Start automatically during boot |
|start|	Start the service now |
| status	 |Inspect current service state |




## Requirements

- WSL2 with Ubuntu 24.04
- systemd enabled
- Docker Engine installed
- Linux user with sudo privileges

## Step 1 – Install OpenSSH Server

**Terminal:** Ubuntu

```bash
sudo apt install openssh-server
```

Verify the SSH client:

```bash
ssh -V
```

Check the server service:

```bash
sudo systemctl status ssh
```

Initially, Ubuntu may show:

```text
Active: inactive (dead)
TriggeredBy: ssh.socket
```

This can occur because Ubuntu uses systemd socket activation for SSH.

## Step 2 – Enable and start SSH

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

Verify:

```bash
sudo systemctl status ssh
```

Expected:

```text
Loaded: loaded (...; enabled; ...)
Active: active (running)
```

## Step 3 – Configure SSH port

By default, SSH listens on port 22.

Our Kamal deployment configuration requires port 2222.

Open the SSH server configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Find:

```text
#Port 22
```

Change it to:

```text
Port 2222
```

Save and exit:

- Ctrl + O → Enter
- Ctrl + X

Validate the configuration:

```bash
sudo sshd -t
```

No output means the configuration is syntactically valid.

## Step 4 – Configure systemd socket activation

Ubuntu 24.04 can use `ssh.socket` to manage the listening port independently of the SSH service configuration.

Therefore, changing `sshd_config` alone may not change the actual listening port.

Create a systemd override:

```bash
sudo systemctl edit ssh.socket
```

Add:

```ini
[Socket]
ListenStream=
ListenStream=2222
```

Explanation:

- Empty `ListenStream=` resets the original socket listener.
- `ListenStream=2222` configures the new listener.

Save and exit.

## Step 5 – Apply the socket configuration

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Restart the SSH socket:

```bash
sudo systemctl restart ssh.socket
```

Check socket status:

```bash
sudo systemctl status ssh.socket
```

Expected:

```text
Active: active (listening)

Listen: 0.0.0.0:2222 (Stream)
        [::]:2222 (Stream)
```

Verify the actual listening port:

```bash
sudo ss -ltnp | grep 2222
```

Expected:

```text
LISTEN 0 4096 0.0.0.0:2222
LISTEN 0 4096 [::]:2222
```

## Step 6 – Generate an SSH key pair

Create the SSH directory:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Generate an Ed25519 key pair:

```bash
ssh-keygen -t ed25519 -C "kamal-wsl-deployment"
```

Accept the default location:

```text
/home/deltax/.ssh/id_ed25519
```

For non-interactive Kamal deployments, use an empty passphrase in this local setup.

This generates two files:

| File | Purpose |
|---|---|
| id_ed25519 | Private key |
| id_ed25519.pub | Public key |

Verify:

```bash
ls -la ~/.ssh
```

Expected:

```text
id_ed25519
id_ed25519.pub
```

**Security:** Never share or commit the private key. Protect it carefully because it has no passphrase.

## Step 7 – Authorize the public key

The SSH server checks the `authorized_keys` file to determine which public keys are allowed to authenticate.

Append the generated public key:

```bash
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

Set permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

Verify:

```bash
ls -la ~/.ssh
```

Expected:

```text
authorized_keys
id_ed25519
id_ed25519.pub
```

## Step 8 – Test key-based SSH authentication

Connect to the local SSH server:

```bash
ssh -p 2222 -i ~/.ssh/id_ed25519 deltax@127.0.0.1
```

On the first connection, SSH asks you to verify the server's host fingerprint.

Enter:

```text
yes
```

This adds the server's host key to `known_hosts`.

Expected:

```text
Welcome to Ubuntu 24.04.5 LTS
```

Verify the connected user:

```bash
whoami
```

Expected:

```text
deltax
```

Exit the SSH session:

```bash
exit
```

## Step 9 – Understand the SSH authentication flow

The authentication flow in our setup:

1. SSH client connects to `127.0.0.1:2222`.
2. SSH server identifies the requested Linux user (`deltax`).
3. Server checks `~/.ssh/authorized_keys`.
4. Client proves ownership of the corresponding private key.
5. Server verifies the proof against the authorized public key.
6. Authentication succeeds without requiring the Linux account password.

## Completion Checklist

- [x] OpenSSH Server installed
- [x] SSH service enabled
- [x] SSH server configuration updated to port 2222
- [x] systemd socket override configured
- [x] SSH socket listening on port 2222
- [x] Ed25519 key pair generated
- [x] Public key added to authorized_keys
- [x] SSH key-based authentication tested
- [x] Local SSH connection successful

## Result

Ubuntu 24.04 inside WSL2 is now accessible through SSH on port 2222 using Ed25519 key-based authentication.

The machine is ready for the next networking configuration required by the Windows-to-WSL2 deployment architecture.

## Important Notes

- SSH server configuration: `/etc/ssh/sshd_config`
- systemd socket override: `/etc/systemd/system/ssh.socket.d/override.conf`
- SSH public-key authorization: `~/.ssh/authorized_keys`
- Private key: `~/.ssh/id_ed25519`
- Public key: `~/.ssh/id_ed25519.pub`
- SSH service: `ssh.service`
- SSH socket: `ssh.socket`

Ubuntu 24.04 may use systemd socket activation. Always verify the actual listening port using `ss -ltnp`, rather than relying only on `sshd_config`.

Membership in the Docker group and possession of an SSH key with access to this account both grant significant privileges. Protect these credentials and restrict access accordingly.
