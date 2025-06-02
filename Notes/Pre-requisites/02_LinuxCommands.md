# Lesson: Most Used Linux Commands

This lesson covers essential Linux commands commonly used in terminal-based environments.

<br>

## 1. File and Directory Management

### List Files

```bash
ls
```

**Output:** Lists files in the current directory.
<br>

### Change Directory

```bash
cd /path/to/directory

cd ..          To go back
```

**Output:** Changes the current working directory.
<br>


### Print Current Directory

```bash
pwd
```

**Output:** Displays the full path of the current directory.
<br>


### Make Directory

```bash
mkdir new_folder
```

**Output:** Creates a new folder named `new_folder`.
<br>


### Remove File or Directory

```bash
rm filename
rm -r foldername
```

**Output:** Deletes files or recursively deletes folders.
<br>


### Copy Files

```bash
cp source.txt destination.txt
```

**Output:** Copies `source.txt` to `destination.txt`.
<br>


### Move or Rename Files

```bash
mv oldname.txt newname.txt
```

**Output:** Renames or moves the file.

<br>
<br>

##  2. File Content and Editing

### Display File Content

```bash
cat file.txt
```

**Output:** Prints the content of `file.txt` to the terminal.
<br>

### View File with Paging

```bash
less file.txt
```


**Output:** Opens file for page-by-page reading.
<br>


### Search Inside Files

```bash
grep 'pattern' file.txt
```

**Output:** Prints lines containing 'pattern'.
<br>


### Edit Files

```bash
nano file.txt
```

**Output:** Opens file in the nano text editor.

<br>
<br>

##  3. System Information

### Current User

```bash
whoami
```

**Output:** Displays the current username.
<br>


### System Info

```bash
uname -a
```

**Output:** Kernel and OS info.
<br>


### Uptime

```bash
uptime
```

**Output:** How long the system has been running.
<br>


### Disk Usage

```bash
df -h
```

**Output:** Shows disk usage in human-readable format.
<br>


### Memory Usage

```bash
free -h
```

**Output:** Displays free and used memory.

<br>
<br>

##  4. Process and Package Management

### View Running Processes

```bash
top
```

**Output:** Interactive display of system processes.
<br>


### Kill a Process

```bash
kill PID
```

**Output:** Terminates process with given PID.
<br>


### Install Package (Debian-based)

```bash
sudo apt install packagename
```

**Output:** Installs a package.
<br>


### Update Packages

```bash
sudo apt update && sudo apt upgrade
```

**Output:** Updates package lists and upgrades installed packages.

<br>
<br>

## 5. Networking

### Show IP Address

```bash
ip a
```

**Output:** Lists network interfaces and IP addresses.

<br>

### Ping a Host

```bash
ping google.com
```

**Output:** Sends ICMP requests to the host.

<br>

### Download File

```bash
wget http://example.com/file
```

**Output:** Downloads file from the URL.

<br>
<br>

## **Directory and File Management:**

* `ls`: Lists files and directories within a specified path.
* `pwd`: Prints the current working directory.
* `cd`: Changes the current directory.
* `mkdir`: Creates a new directory.
* `rmdir`: Removes an empty directory.
* `rm`: Removes files or directories.
* `cp`: Copies files or directories.
* `mv`: Moves or renames files or directories.
* `touch`: Creates an empty file or updates the timestamp of an existing file.
* `code filename.extension`

**File Content Management:**

* `cat`: Displays the content of a file.
* `less`: Views file contents interactively.
* `head`: Displays the first lines of a file.
* `tail`: Displays the last lines of a file.
* `grep`: Searches for specific patterns within files.
* `find`: Searches for files and directories based on given criteria. 

**Permissions and Ownership:**

* `chmod`: Changes file access permissions.
* `chown`: Changes file ownership.
* `chgrp`: Changes group ownership of files. 

**System Information:**

* `uname`: Displays system information.
* `df`: Displays free disk space.
* `du`: Displays disk space usage.
* `ps`: Lists currently running processes.
* `top`: Displays real-time process information. 

**Networking:**

* `ping`: Tests network connectivity.
* `ssh`: Provides secure remote login.
* `scp`: Securely copies files between systems.
* `wget`: Downloads files from the web. 

**Other Useful Commands:**

* `man`: Displays manual pages for commands.
* `history`: Displays command history.
* `alias`: Creates aliases for commands.
* `su`: Switches to another user account.
* `sudo`: Executes a command with superuser privileges.
* `tar`: Creates and extracts archives.
* `gzip`: Compresses files.
* `gunzip`: Decompresses files.
* `zip`: Creates zip archives.
* `unzip`: Extracts files from zip archives. 

---
> Mastering these commands is fundamental to becoming proficient in Linux system administration.
---
