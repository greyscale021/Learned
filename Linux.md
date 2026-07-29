# Linux

<details>
    <summary><strong>Index</strong></summary>

1. [Linux navigation](#1-linux-navigation-commands)
2. [File management](#2-file-management)
3. [Permissions & ownership](#3-permissions--ownership)
4. [Processes & System Control](#4-processes--system-control)
5. [SSH (Secure Shell)](#5-ssh-secure-shell)
6. [Package Management & File Transfer ](#6-package-management--file-transfer)
7. [Shell Redirection & Operators](#7-shell-redirection--control-operators)
8. [Text Processing: grep, sed, awk](#8-text-processing--grep-sed-awk)
9. [Vim Basics](#9-vim-basics)
10. [Cron Jobs](#10-cron-jobs)
11. [Zombie Processes](#11-zombie-processes)
12. [Networking Commands](#12-networking-commands)
</details>

<details>
    <summary><strong>Things to be clear about</strong></summary>

1. The fundamental structure of commands in linux:<br>
    - `cmd -[flags] [source] [destination]`
    - or, `cmd -[flags] [target] [destination]`
    - flags: options, that are preceded by dash (-) or two dashes (--) for spelled-out
3. Combining Options: Single-character options can be combined after a single dash (e.g., ls -la). 
4. sudo (superuser do) - makes you an admin for short period
5. Process and Service are different things in linux
 - Process: A running instance of a program
 - Service: A background program managed by the system 

</details>


## 1. Linux Navigation Commands

These are the fundamental navigation commands in Linux.


###  `pwd` (Print Working Directory)

```bash
pwd 
# prints current directory
```


### `ls` (List)

```bash
ls      # lists files in the current directory
ls <folder> # lists file in the <folder>

# With flags-
ls -l   # lists in long format(permissions, owner, size, date)
ls -a   # lists hidden (files starting with .)
ls -h   # lists in human readable
ls -R   # lists in recursive (shows subdirectories of folders)

# Merging flags-
ls -la  # lists long foramted hidden files
ls -lah # long format + hidden + human readable
```


### `cd` (Change directory)

```bash
cd <folder> # change directory to <folder>

cd ..      # go up/back one level
cd ~       # go to home directory (default cd)
cd /       # Go to root directory
cd -       # Go to last/previous directory
```

Paths are two types:

```bash
# Absolute Path (full path from root)
cd /home/user/Documents

# Relative Path (from current directory)
cd Documents
```

### `tree` (installation needed)

```bash
tree # displays directory structure visually
```


### `find` (search for files)

```bash
find . -name "<file>" 
# find <file> in current directory 

# structure: find [starting-point] [expression]

# starting-point: current directory ".", root "/", home "~"
```

### `locate`

```bash
# locate is fast file search (uses an indexed db)
# need to update the database for 'locate'

sudo updatedb   # to update the db
locate <file>     # to use locate, finds <file>
```

### `stat` (Status)

```bash
stat <file>
# shows detailed file information about <file>
# size, permissions, ownership, access
```


### `realpath`

```bash
realpath <file> 
# shows the absolute path of <file>
```


### `clear`

```bash
clear 
# clears the terminal screen
# (ctrl + L) shortcut; moves the texts up
```
### `pushd`/`popd` (push/pop directory)
```bash
pushd <directory> 
# saves current directory in stack and moves to <directory>

popd 
# pops(apply and delete) the top directory from the stack (LIFO)

dirs 
# displays the current directory stack
```

---
## Advance navigation

### `history` (command history)
```bash
history     
# lists history of commands

history <number>    
# lists last <number> commands

!<number>  
# run <number> numbered command from history
!!      
# runs last command
!<command>  
# runs most recent command starting with <command>

history -c  
# clears the cmd history
```

### `whatis`
```bash
whatis <command> 
# explanation of <command> in one sentence
```

### `type`
```bash
type <command> 
# prints <command> classification
```

### `which`
```bash
which <command>
# lists the exact executable that will run for <command>
```

### `whereis`
```bash
whereis <command> 
# lists all known locations of a <command>
```

### `man` (Manual)
```bash
man <command> 
# opens the official manual for <command>
```
### `alias`
```bash
alias <new_name>='<command>'
# makes an alias of <command> as <new_name>

alias
# lists all alias

unalias <name>
# removes the alias of <name>
```
---
### Important Directories ~~

- `/home` - user directories
- `/root` - root user’s home 
- `/etc`  - system configuration files  
- `/var/log` - system logs  
- `/bin`  - command binaries  
- `/usr`  - installed applications and libraries

- `/tmp`  - temporary files
- `/dev`  - devices
- `/proc` - running process info (virtual filesystem)
- `/opt`  - optional software
- `/lib`  - essential shared libraries

### Reading logs
```bash
ls /var/log
# prints what log files exist
find /var/log -name "*.log"
# find all files with .log, in /var/log

tail -f /var/log/syslog        
# live system logs

cat /var/log/syslog | grep ssh 
# print logs with filer for specific service, here ssh
```
---

## 2. File Management


### Create/Delete-
### `touch` (Create File)
```bash
touch <file>
# if <file> is new, creates an empty file
# if already exists it will update timestamp of <file>

touch file_{a..c}.txt
# will create a.txt,b.txt and c.txt

touch -c <file> 
# changes just the timestamp
touch -a <file>
# changes just the access time
touch -m <file> 
# changes just the modification time
```
### `mkdir` (Make directory)
```bash
mkdir <folder>  
# creates <folder> directory
mkdir folder1 folder2 
# creates multiple directories

mkdir -p a/b/c  
# creates nested directories
# -p (parent)
```
### `rmdir` (Remove directory)
```bash
rmdir <folder>  
# removes <folder> (if empty)
```
### `rm` (Remove)
```bash
rm <file>          
# deletes <file>

rm -r <folder>    
# deletes <folder> recursively
# for using rm on folder

rm -f <file>  
# force delete <file>
rm -rf <folder>   
# forced recursively removes folder
```
---
### Copy/cut-
### `cp` (Copy)
```bash
cp <source> <destination>
# copy from <source> to <destination>

cp -r folder1 folder2
# copy directory recursively(for folders)

# if folder2 is new:
# will copy folder1 contents and create folder2 with it

# if already existed:
# will create folder2/folder1 directory
```
### `mv` (Move / Cut)
```bash
mv <source> <destination>  
# move from source to destination path

mv file.txt newname.txt 
# also can be used as renaming tool
```
```bash
mv demo.txt "$(cd .. && pwd)"/demo.txt
# moves demo.txt file one level back
```
---
### Archive-
### `zip`(Package and compress files)
```bash
zip file.zip file1 file2
# zip file1, file2 as file.zip

# it isn't reverse than normal linux commands
# zip just see .zip as target or source

zip -r <name.zip> <directory>
# zip <directory> as <name.zip>
```
### `unzip`(Extract)
```bash
unzip archive.zip
# extract archive.zip
unzip archive.zip -d /path
# extract to a specific directory

unzip -l archive.zip
# list contents without extracting
```
---
### View-
### `cat` (Display / Concatenate)
```bash
cat <file>  
# display contents of <file>
cat <file1> <file2>    
# concatenate two files as stdout
# display contents of <file1> + <file2>
```
### `less` (Display in pages)
```bash
less <file> 
# display content of file with pagination

# q: quit | j/k: scroll down/up
# space: next page
# /: search | n: next match
```
### `head`
```bash
head <file> 
# display the first few lines of <file>
```
### `tail`
```bash
tail <file> 
# display the last few lines of <file>
tail -f <file>  
# -f (follow), tails in realtime
# used for logs like "tail -f /var/log/syslog"
```
### `grep` (filter)
```bash
grep "<name>" <file>
# displays content with filter <name> of <file>
grep -r "<name>" <directory>  # grep of folders

# usecase:
# grep "error" server.log
# grabs all error logs
```
### `ln` (Link)
```bash
ln <file1> <file2> 
# creates a hard link of file1 as file2 
# hard link: both shares the same data and memory
```
### `nano` (a text editor)
```bash
nano <file.txt> 
# opens file in nano editor
# ctrl + o + enter: save
# ctrl + x: exit
```
---
## 3. Permissions & Ownership

Permissions control read, write, and execute access of files.

Who:
- user (u): the person who owns the file
- group (g): group of users that are associated with the file
- others (o): all other users

Permissions:
- `r` (read: view contents of the file)
- `w` (write: modify the file)
- `x` (execute: run the file as a program)
- `-` (null)
--- 
### Permission Control-
### Viewing permissions:
### `ls -l` (list in long format)
```bash
ls -l <file>
# will list extra details of <file>
# demo: -rwxr-xr-- 2 pokemon pokeball 1024 Feb 29 file.txt
```
``` 
# first column: file type and permissions

# file_type: regular_file(-), directory(d), link:(l)
# permissions: displayed in 3 sections of per 3 characters
# sections: 1st(owner), 2nd(group) and 3rd(others)

# second : number of hard links
# third : user name
# fourth : group name
# fifth : file size
# sixth : date and time
# last : file name
```
Example:
```bash
```bash
ls -l file.txt
# output: -rwxr-xr-- 1 user group 1234 Jan 1 file.txt

# file type: - (regular file)
# permissions:owner(rwx), group(r-x), others(r--)
# hard links:1
# user: user, group: group
# size: 1234 and date jan 1, file name: file.txt
```

### Changing permissions:
### `chmod` (Change mode)
Controls file/directory permissions. 

Numeric mode:
```bash
chmod <permissions> <file>
# here values, r:4, w:2, x:1 
# rwx = 7, rw- = 6, r-x = 5, r-- = 4, --- = 0

# chmod 754 file.txt
# user: rwx, group: r-x, others: r--)

chmod -r <permissions> <dir>
# chmod of folder
```
Symbolic mode:
```bash
chmod [who][operators][permissions] <file>
# who: u-user, g-group, o-others, a-all
# operators: add(+), remove(-), set(=)
# permissions: r-read, w-write, x-execute

# chmod u+w,g+wx,o= file.txt
# add write permission to user
# add write + execute permission to group
# set null permission to others

# chmod +x script.sh
# add execute permission to all
# equivalent:chmod u+x,g+x,o+x script.sh
```
---
### Ownership control-
### Viewing ownership:
### `who`, `read` and `id`
```bash
users   
# displays logged in users name

who
# detailed version of read
# displays logged in users name+login_time

w
# detailed version of who
# displays logged in users name+login_time+activity

getent passwd  # To see all users
```
### `id` (compact who)
```bash
id
# display permissions and groups of all users
id <user>
# display permissions and groups of <user>
```
### `groups`
```bash
groups  # All groups
cat /etc/group  # To see all group details
```

### `Change`-
### `chown` (change owner)
```bash
chown <owner>:<group> <file> # Change the owner and group of the file to <owner> and <group>
chown <owner> <file>    # Just change the <owner> of <file>
chown -R <owner>:<group> <directory> # Recursively change the owner and group of the directory to <owner> and <group>
```


## Managing users and groups
### `useradd` (Add user)
```bash
useradd <username>  # Creates a user
useradd -m <username>  # Creates a user and a user directory
adduser     # Automatically adds default flags to useradd

sudo userdel <username> # Deletes a user
sudo userdel -r <username>  # Deletes a user and their files
```
### `su` (Switch user)
```bash
su <username>  # Switch to <username> (keeps current env)
su - <username> # Switch to <username> (Loads their full environment)
su -c <cmd> <user> # Run a single command as another user
sudo -i -u <username>  # Switch to another user (without needing their password if you're root or have sudo permissions)
```

### `passwd` (Password)
```bash
passwd      # Set the password for the current user
passwd <username>       # Set the password for the <username>
passwd -S [username]    # displays if an account is locked or active (P- password set, L- locked, NP- no password)
passwd -l [username]    # disables a password
passwd -u [username]    # re-enables a password
passwd -d [username]    # removes the password
```

### `groupadd` (Add group)
```bash
groupadd <groupname>  # Create a group
groupdel <groupname>  # Deletes a group
```
### `usermod` (Modify user)
```bash
usermod -aG <group> <user>   # add user to group (-a = append)
gpasswd -d <user> <group>    # Removes <user> from <group>

usermod -L <user>            # lock account
usermod -U <user>            # unlock account
usermod -d /new/home <user>  # change home directory
```

## 4. Processes & System Control
## Process Control-
### `Monitoring`:
### `top`
```bash
top     # Opens a real-time system monitoring tool (like task-manager)
# q - quit, k - kill process by pid, shift + (p - sort by cpu, m - sort by memory, v - parent tree view)
htop    # Enhanced version of top (required installation)
```
### `ps` (Processes)
```bash
ps      # Shows processes running in the current shell
ps aux  # a-all user | u-user oriented | x-include ps without terminal (daemons)
ps -ef  # e- every process | f- full format (normally used in scripting)
ps -u <username>    # List processes for <username>
```
```bash
pgrep <process> # PID grep of <process>
```
`Start process with priority`-
### `nice`
```bash
nice -n <priority> <command>  # Start a process with higher/lower priority
# Lower the n value, higher the priority (default n value 0, lowest -20, highest 19)
```
### `renice`
```bash
renice <priority> -p <PID>  # Change priority of an existing process
```

### `Ending`:
### `kill`  (End/terminate process by PID (process id))
```bash
kill <PID>      # Kill process by PID
kill -9 <PID>   # Force kill a process (SIGKILL)
```
### `pkill` (Kill processes by name)
```bash
pkill <process-name>    # Kill process by name
```
### `killall` (Kill all processes by name)
```bash
killall <process_name>  # Kill all processes by name
```

### `Process` (Background/foreground)-
### `jobs` (Process that was started from the current session)
```bash
jobs # See all the current jobs and job Ids
```
### `bg` (Background)
```bash
bg <job-id>     
# Send process to background
# Use ctrl+z to suspend the process and display its job number
bg %<job-id>
# Resumes the execution in the background
```
### `fg` (Foreground)
```bash
fg <job_id> # Bring a job to the foreground
```

## System Control-
### `Monitoring`-
### `uname`
```bash
uname -a    # Display all system information in one line
```
### `uptime`
```bash
uptime       # Display uptime, load average, and number of users
```
### `iostat` (Input-output status)
```bash
iostat       # Show CPU and I/O statistics
```
### `free`
```bash
free -h      # Show RAM usage in human-readable format
```
### `df` (Disk free) - Disk space of the whole filesystem
```bash
df         # Shows disk space usage of mounted filesystems
df -h      # Human-readable format
df -T	   # Type of file system (ext4, xfs, tmpfs)
df .	   # Info of the disk containing the current folder
```
### `du` (Disk usage) - Size of specific files or directories
```bash
du               # Shows the disk used by directories(current, subs and . total)
du -h  <dir>     # Human-readable
du -sh <dir>     # Total size of a directory

du -ah <dir>     # Displays sizes for every directory and file
```
### `lsblk` (List block)
```bash
lsblk       #   List information about all available block devices(hd,ssd,partitions etc)
```


### `Managing`-
### `systemctl` (System control)
```bash
systemctl status <service_name>   # Check the status of a service

systemctl start <service_name>    # Start a service
systemctl stop <service_name>     # Stop a service
systemctl restart <service_name>  # Restart a service

systemctl enable <service_name>   # Enable service to start at boot
systemctl disable <service_name>  # Disable service from starting at boot
```
### `journalctl` (diagnostic tool)
```bash
journalctl -u <service>        # logs for a specific service
journalctl -u <service> -n 5   # last 5 lines
journalctl -xe                 # most recent logs with context (use after a crash)
journalctl -f                  # follow logs in real time
journalctl --since "<time>"    # <time> can be: "10 min ago","2026-03-07 10:30:00", "yesterday"
journalctl -p err       # Only show errors

# When a service fails: 
# systemctl status <service>  (quick snapshot)
# journalctl -xe  (full picture)
# journalctl -u <service> -n 50  (Look into that service)
# journalctl -b -1  (To look into boot)
```

### `mount`
```bash
mount   # List mounted file systems
mount <source> <target>   # Mount source to target
```
### `umount` (Unmount)
```bash
umount <directory>   # Unmount the file system from <directory>
```

### `power`-
### `shutdown`
```bash
shutdown -h now   # Shutdown immediately
shutdown -h +10   # Shutdown in 10 minutes
shutdown -r now   # Reboot immediately
shutdown -r 22:00 # Reboot at 10:00 PM

shutdown -c       # To cancel the schedule
```
### `reboot`
```bash
reboot      # Reboot immediately
```
### `poweroff`
```bash
poweroff    # Poweroff immediately
```
### `halt`
```bash
halt    # Stops the os, not the machine
```

# 5. SSH (Secure Shell)

SSH lets you securely connect to and control a remote machine(usually a server) over a network. All communication is encrypted.

```bash
ssh user@host          # Connect to a remote machine
#  user(a user on the server), host(server ip address)(or domain name)
ssh user@ip            # Connect using IP address
ssh -p 2222 user@host  # Connect on a specific port (default is 22)
exit                   # Close the connection (or Ctrl+D)
```

---

## Key Concepts

**Password login**: You type a password every time. It travels over the network encrypted. Risk: brute force, weak passwords, credential leaks.

**Key-based login**: Generate a key pair. The private key stays on your machine. The public key goes on the server. The server locks a challenge with your public key only your private key can unlock it. It's fundamentally safer because no credential crosses.

- **Public key** (`id_ed25519.pub`): Safe to share, goes on the server. (treat it as username)
- **Private key** (`id_ed25519`): Never leaves the machine. (treat it as password)

---

## `~/.ssh/` Directory Structure

```bash
~/.ssh/
|-- id_ed25519          # your private key (chmod 600)
|-- id_ed25519.pub      # your public key
|-- authorized_keys     # public keys allowed to log in (lives on the server)
|-- known_hosts         # servers you've connected to before (fingerprints)
 `- config              # optional: shortcuts for ssh connections

# ssh = client tool     sshd = server daemon
```

| File | Lives on | Purpose |
|---|---|---|
| `id_ed25519` | Client | private key|
| `id_ed25519.pub` | Client | public key|
| `authorized_keys` | Server | Who is allowed to log in |
| `known_hosts` | Client | Servers you've verified before |
| `sshd_config` | Server | SSH server configuration |

> **authorized_keys vs known_hosts:**
> `authorized_keys` = server authenticating **you**.
> `known_hosts` = you authenticating the **server** (prevents man-in-the-middle).

>**Permissions**: SSH refuses to work if your private key is too open

```bash
chmod 700 ~/.ssh        # Change the permission to user: read, write and execute
chmod 600 ~/.ssh/id_ed25519     # Change the permission to user: read and write
```

If you see `WARNING: UNPROTECTED PRIVATE KEY FILE!` - run the above. SSH protecting itself from exposed private keys is a core feature.

---

## Full Process (Connecting to a Remote Server)

### Step 1: Prepare the Server

The server needs `sshd` running to accept connections.

```bash
sudo apt install openssh-server -y
sudo systemctl enable ssh   # start on boot
sudo systemctl start ssh    # start now
sudo systemctl status ssh   # confirm status
```

### Step 2: Connect With Password (First Time)

From the server:

Get the server's IP and username from the server machine and check if password authentication is enabled.

```bash
ip a       # find the 192.168.x.x address
whoami     # confirm the username
sudo nano /etc/ssh/sshd_config  # Find "PasswordAuthentication"
```

From the client:

```bash
ssh username@192.168.x.x
```

First connection → server sends fingerprint → you type `yes` → saved to `known_hosts` → enter password(the server machines)

### Step 3: Set Up Key-Based Login

On the client, generate a key pair:

```bash
ssh-keygen -t ed25519   # Press Enter to accept default save location (~/.ssh/id_ed25519)
# Passphrase is optional but adds an extra layer of protection
ssh-keygen -t ed25519 -f /path/to/directory/key_name    # -f flag for selecting path manually 
```

Copy your public key to the server:

```bash
ssh-copy-id -i ~/path_to_public_key username@192.168.x.x
# Appends your public key to the server's ~/.ssh/authorized_keys
```

Manual equivalent (if ssh-copy-id isn't available):

```bash
cat ~/.ssh/path_to_public_key | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

Now connect without password:

```bash
ssh username@192.168.x.x
```

### Step 4: Harden the Server

Once key login works, disable password login:

```bash
sudo nano /etc/ssh/sshd_config
```

```
PasswordAuthentication no    # disable password login
PubkeyAuthentication yes     # keep key auth on
PermitRootLogin no           # never allow direct root login
```

```bash
sudo systemctl restart sshd
```

> Note: Always test in a **new terminal** before closing your current session. If key login fails after disabling passwords, you'll lock yourself out.

---

## Using the Server Remotely

```bash
# Full interactive session
ssh username@ip

# Run a single command without fully logging in
ssh username@ip "uptime"
ssh username@ip "df -h"

# Watch SSH logs live on the server (tie this together with journalctl)
journalctl -u ssh -f
```

---

## The `config` File

Instead of typing `ssh user@192.168.x.x` every time:

```bash
# sudo nano ~/.ssh/config
Host <name>     # The name you want to select
    HostName <192.168.x.x>
    User <username>     # Username
    Port 22             # Default port
    IdentityFile ~/path_to_private_key
```

Now just type:

```bash
ssh <name>      # You will be logged in, equivalent to "ssh user@ip"
```

Essential when managing multiple servers.

---

## The Passphrase + `ssh-agent`

`ssh-keygen` asks for a passphrase:

- **Without passphrase:** Anyone who gets your private key file can use it immediately.
- **With passphrase:** Even if someone steals your private key file, they still need the passphrase to unlock it.

The downside is typing it every time, solved with `ssh-agent`:

```bash
eval "$(ssh-agent -s)"      # start the agent
ssh-add ~/.ssh/id_ed25519   # add your key (type passphrase once)
# now SSH without typing the passphrase again for the rest of the session
```

---

## Key Types

```bash
ssh-keygen -t rsa -b 4096    # rsa (old standard)
ssh-keygen -t ed25519        # ed25519 (modern)
```

| Type | Key Size | Speed | Security |
|---|---|---|---|
| RSA | 4096 bits | Slower | Good |
| Ed25519 | 256 bits | Much faster | Better |

Ed25519 is based on modern elliptic curve math. A 256-bit Ed25519 key is more secure than a 4096-bit RSA key.

---

## How the Handshake Works

```
1. Client connects to server on port 22
2. Server sends its fingerprint
   └── In known_hosts? → auto-accept | First time? → you type yes → saved
3. Client offers public key
4. Server checks authorized_keys → finds it
5. Server encrypts a random challenge with your public key → sends it
6. Client decrypts it with private key → sends back proof
7. Server confirms → only the real private key could do that → access granted
```

The private key never crosses the network.

---

## Common Errors

| Error | Meaning | Fix |
|---|---|---|
| `Permission denied (publickey)` | Server doesn't have your public key | Run `ssh-copy-id` |
| `WARNING: UNPROTECTED PRIVATE KEY FILE` | Key permissions too open | `chmod 600 ~/.ssh/id_ed25519` |
| `Host key verification failed` | Server fingerprint changed | Verify server, update `known_hosts` |
| `Connection refused` | sshd not running or wrong port | Check `systemctl status ssh` on server |
| `Connection timed out` | Firewall blocking port 22 or wrong IP | Check IP and firewall |

---

## Why This Matters for DevOps

SSH is the backbone of everything remote:

- Connecting to EC2 instances on AWS
- Ansible (runs over SSH under the hood)
- Git over SSH
- CI/CD pipelines authenticating to servers
- Port forwarding and tunneling

---

## 6. Package Management & File Transfer
A package manager handles installing, updating, and removing software on Linux. Different distros use different ones.
## Package Management-
### `apt` (Advanced pakage tool)
```bash
apt update              # Refresh the package index (always run before install)
apt upgrade             # Upgrade(update) all installed packages, using reference of pakage index
apt full-upgrade        # Upgrade everything that were already fetched using, update, auto remove old packaged if necessary
apt install <package>   # Install a package
apt remove <package>    # Remove a package (keeps config files)
apt purge <package>     # Remove a package and its config files
apt autoremove          # Remove unused dependencies
apt search <package>    # Search for a package
apt show <package>      # Show package details
```

### `snap`  (Cross-distro containerized apps) - Snap packages are self-contained. They bundle their own dependencies.

```bash
snap install <package>      # Install a snap package
snap remove <package>       # Remove a snap package
snap list                   # List installed snaps
snap refresh                # Update all snap packages
```

## Archiving & Compression

In linux, 
- Archiving- bundles files together. 
- Compression- reduces their size. 

`tar.gz` is standard on Linux servers. `zip` is useful for cross-platform sharing.

### `tar` (Tape Archive)- For archiving or compressing a directory
Flags-
- c - create / write files into a new .tar
- x - extract / unpack an archive
- v - verbose / Print every file name while processing it.
- t - list contents 
- f - file name
- z - gzip / Pipe through gzip(GNU zip)(compress) automatically

```bash
tar -cvf archive.tar folder/        # Create an archive
tar -xvf archive.tar                # Extract an archive
tar -czvf archive.tar.gz folder/    # Create a gzip-compressed archive
tar -xzvf archive.tar.gz            # Extract a gzip-compressed archive
tar -tf archive.tar                 # List contents without extracting
```

### `gzip`/`gunzip` - For compressing a file

```bash
gzip <file>           # Compress file (replaces original with <file.gz>)
gzip -k <file>        # Compress and keep the original
gunzip <file>         # Decompress
gzip -d <file>.gz     # Same as gunzip
```

## Networking & File Transfer

### `ping`  (Useful for quickly checking network connectivity and latency)
```bash
ping <host>             # Test if a host is reachable
ping -c 4 google.com    # Send exactly 4 packets then stop
ping -i 2 google.com    # Ping every 2 seconds
```
### `curl` (Client URL)
```bash
curl <url>  # Fetch content from a URL (outputs to stdout)
curl -o file.html <url> # Save output to a file
curl -I <url>   # Fetch only the HTTP headers
curl -X POST -d "data" <url>    # Send a POST request
curl -H "Authorization: Bearer <token>" <url>  # Send request with a header
curl -s <url>   # Silent mode (no progress bar)
```
> In DevOps used for API calls, health checks, and testing endpoints.

### `wget` (Web Get)

```bash
wget <url>                          # Download a file from a URL
wget -O output.zip <url>            # Save with a specific filename
wget -c <url>                       # Resume an interrupted download
wget -q <url>                       # Quiet mode
wget -r <url>                       # Recursive download (entire site)
```

> `wget` is better for downloading files. `curl` is better for API interaction.

---

### `scp` (Secure Copy — over SSH)

```bash
scp file.txt user@host:/path/       # Copy local file to remote server
scp user@host:/path/file.txt .      # Copy remote file to current directory
scp -r folder/ user@host:/path/     # Copy entire directory recursively
scp -P 2222 file.txt user@host:~/   # Use a non-default SSH port
```

> `scp` uses SSH under the hood — same keys, same port, same security.

---

### `rsync` (Remote Sync)
Flags -
- a - archive (preserves permissions, timestamps)
- v - verbose
- z - compress
 

```bash
rsync -av source/ user@host:/dest/      # Sync local folder to remote
rsync -av user@host:/source/ dest/      # Sync remote folder to local
rsync -av --delete source/ dest/        # Mirror (delete files not in source)
rsync -avz source/ user@host:/dest/     # Compress during transfer (-z)
```

> `rsync` only transfers changed files, making it far more efficient than `scp` for repeated syncs. Standard in DevOps for deployments and backups.

## 7.Shell Redirection & Control Operators


These operators control:

- Data flow (where output goes)
- Execution flow (when commands run)

Linux commands communicate using:
- stdin  (standard input)
- stdout (standard output)
- stderr (standard error)

## Redirection Operators (Data → File)

`>` (Redirect stdout — overwrite)
```bash
echo "hello" > file.txt # take the output of echo and overwrite the file.txt
```

`>>` (Redirect stdout — append)
```bash
echo "hello" >> file.txt # Appends output to the end of the file.
```

`2>` (Redirect stderr)
```bash
command 2> file # Redirects error output to a file.
```

`&>` (Redirect stdout + stderr)
```bash
command &> file #Redirects both normal output and errors into one file.
```

## Pipe Operator (Data → Command)

`|` (Redirect stdout data → Command stdin)
```bash
ps aux | grep python 
# Redirects the stdout of the left to the stdin of the right cmd.
```
## Substitution operator (stdout → as command)
`$()` (Redirect stdout as command)
```bash
$(command)      
# Runs command and substitutes its output directly into the shell line
# As if user typed it in the shell
```

## Control Operators (Execution flow)

`&&` (Logical AND)
```bash
command1 && command2    # Runs the next command if the previous succeeds
```

`||` (Logical OR)
```bash
command1 || command2    # Runs the next command if the previous fails
```

`;` (Sequential Execution)
```bash
command1 ; comman2  # Runs command sequentially regardless of success or failure.
```
---

## 8. Text Processing — grep, sed, awk

These three tools are the Unix text-processing pipeline. Together they let you search, transform, and extract structured data from logs and config files — a core DevOps skill.

---

### `grep` — Extended Patterns for Log Work

Basic usage already covered in Section 2. Extended patterns:

```bash
grep 'ERROR' app.log                  # Lines containing ERROR
grep -i 'error' app.log               # Case-insensitive
grep -n 'ERROR' app.log               # Show line numbers
grep -c 'ERROR' app.log               # Count matching lines
grep -v 'DEBUG' app.log               # Invert: lines NOT matching
grep -A 3 'ERROR' app.log             # Print 3 lines AFTER match
grep -B 2 'ERROR' app.log             # Print 2 lines BEFORE match
grep -E 'ERROR|WARN' app.log          # Extended regex (either match)
grep -r 'AWS_SECRET' /etc/            # Recursive through directories

# Real log work:
grep 'ERROR' /var/log/nginx/error.log | tail -50
grep -E '^2026-04' app.log | grep 'FAIL'   # Errors in April 2026
```

---

### `sed` — Stream Editor (Find & Replace)

`sed` reads line-by-line and applies editing commands. Most commonly used for substitution.

```bash
# Basic substitution: s/find/replace/
sed 's/old/new/' file.txt             # Replace FIRST match per line
sed 's/old/new/g' file.txt            # Replace ALL matches per line (global)
sed 's/old/new/gi' file.txt           # Case-insensitive global replace

# In-place editing (edit the file directly):
sed -i 's/old/new/g' file.txt         # Edit in place
sed -i.bak 's/old/new/g' file.txt     # Edit in place, keep .bak backup

# Other uses:
sed -n '10,20p' file.txt              # Print lines 10-20 only
sed '/^#/d' config.txt                # Delete comment lines
sed '/^$/d' file.txt                  # Delete blank lines

# Real DevOps use: update config during provisioning
sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
```

---

### `awk` — Pattern & Field Processor

`awk` processes text column by column. Each field is `$1`, `$2`, `$3`... `$NF` is the last field. Think of it as a mini programming language for tabular data.

```bash
# Basic structure: awk 'pattern { action }' file

awk '{print $1}' file.txt             # Print first column of every line
awk '{print $1, $3}' file.txt         # Print columns 1 and 3
awk '{print NR, $0}' file.txt         # Print line number + full line

# Field separator (-F):
awk -F':' '{print $1}' /etc/passwd    # : is separator, prints usernames
awk -F',' '{print $2}' data.csv       # Parse CSV, print second column

# Pattern matching:
awk '/ERROR/ {print $0}' app.log      # Print lines matching ERROR
awk '$3 > 100 {print $0}' data.txt    # Print lines where 3rd col > 100

# Aggregation:
awk '{sum += $1} END {print sum}' nums.txt  # Sum a column of numbers
awk 'END {print NR}' file.txt               # Count total lines

# Real log analysis: extract IP and HTTP status from nginx access log
awk '{print $1, $9}' /var/log/nginx/access.log | grep ' 5'
```

---

### Combining the Three — Log Processing Pipeline

The real power is piping them together:

```bash
# Count ERROR vs WARN lines:
grep -E 'ERROR|WARN' app.log | awk '{print $NF}' | sort | uniq -c | sort -rn

# Top 10 IPs hitting your server:
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10

# Failed SSH login attempts by IP:
grep 'Failed password' /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn

# Replace a value in every config file in a directory:
find /etc/myapp -name '*.conf' | xargs sed -i 's/localhost/10.0.0.5/g'
```

---

## 9. Vim Basics

Vim is the editor you'll use when nano isn't available — common on minimal cloud servers and containers. Knowing the basics is non-negotiable for DevOps.

---

### The Modal System

Vim has modes. This is the thing that trips everyone up. You must know which mode you're in.

| Mode | How to enter | What it does |
|---|---|---|
| Normal | `Esc` from anywhere | Default. Navigate, delete, copy. |
| Insert | `i` (before cursor) / `a` (after cursor) | Type text. |
| Visual | `v` (char) / `V` (line) / `Ctrl+v` (block) | Select text. |
| Command | `:` from Normal mode | Run commands (save, quit, search-replace). |

---

### Essential Commands

```bash
# Opening / quitting:
vim file.txt        # Open file
:q                  # Quit (if no changes)
:q!                 # Quit WITHOUT saving (force)
:w                  # Save (write)
:wq                 # Save and quit
ZZ                  # Save and quit (Normal mode shortcut)

# Entering Insert mode:
i                   # Insert BEFORE cursor
a                   # Insert AFTER cursor
I                   # Insert at beginning of line
A                   # Insert at end of line
o                   # Open new line BELOW and insert
O                   # Open new line ABOVE and insert

# Navigation (Normal mode):
h j k l             # Left / Down / Up / Right
w                   # Jump forward one word
b                   # Jump back one word
0                   # Start of line
$                   # End of line
gg                  # First line of file
G                   # Last line of file
:<n>                # Jump to line number (e.g. :42)
Ctrl+d / Ctrl+u     # Scroll half-page down / up

# Editing (Normal mode):
x                   # Delete character under cursor
dd                  # Delete (cut) entire line
yy                  # Yank (copy) entire line
p                   # Paste below cursor
u                   # Undo
Ctrl+r              # Redo
cw                  # Change word (delete word, enter Insert mode)
.                   # Repeat last action

# Search:
/searchterm         # Search forward
?searchterm         # Search backward
n                   # Next match
N                   # Previous match

# Find and replace (Command mode):
:%s/old/new/g       # Replace all in file
:%s/old/new/gc      # Replace all, confirm each
:10,20s/old/new/g   # Replace in lines 10-20 only
```

---

### Survival Workflow

If you land on a server and need to edit a config:

```
1. vim /etc/ssh/sshd_config   → open the file
2. /PasswordAuth              → search for the setting
3. n                          → jump to next match if needed
4. i                          → enter Insert mode
5. (make your edit)
6. Esc                        → back to Normal mode
7. :wq                        → save and quit
```

---

## 10. Cron Jobs

`cron` is the Unix task scheduler. A daemon (`crond`) reads a crontab and fires jobs at defined times. Critical in cloud/DevOps for log rotation, backups, health checks, and certificate renewal.

---

### Crontab Syntax

```
# ┌───────── minute       (0-59)
# │ ┌─────── hour         (0-23)
# │ │ ┌───── day-of-month (1-31)
# │ │ │ ┌─── month        (1-12)
# │ │ │ │ ┌─ day-of-week  (0-7, 0 and 7 = Sunday)
# │ │ │ │ │
# * * * * *   command-to-run

30 2 * * *    /usr/bin/backup.sh        # every day at 02:30
*/5 * * * *   /usr/bin/health_check.sh  # every 5 minutes
0 0 1 * *     /usr/bin/monthly.sh       # 1st of every month at midnight
0 9 * * 1-5   /usr/bin/workday.sh       # 9am Mon-Fri
@reboot       /usr/bin/start_agent.sh   # on every system boot
```

---

### Managing Crontabs

```bash
crontab -e              # Edit your crontab (opens in $EDITOR)
crontab -l              # List your current crontab
crontab -r              # Remove your crontab (careful!)
crontab -u <user> -l    # View another user's crontab (as root)

# System-wide cron directories (run as root):
ls /etc/cron.d/         # Drop-in cron files
ls /etc/cron.daily/     # Scripts run daily
ls /etc/cron.weekly/    # Scripts run weekly
ls /etc/cron.monthly/   # Scripts run monthly
```

---

### Log Rotation with logrotate

`logrotate` is the standard tool for rotating, compressing, and deleting old logs. Cron triggers it daily.

```bash
# Config example: /etc/logrotate.d/myapp
/var/log/myapp/*.log {
    daily           # rotate every day
    rotate 7        # keep 7 old copies
    compress        # gzip old logs
    missingok       # don't error if log is missing
    notifempty      # don't rotate if empty
    create 0644 root root
}

# Test logrotate manually:
sudo logrotate -d /etc/logrotate.d/myapp   # dry run
sudo logrotate -f /etc/logrotate.d/myapp   # force rotate now
```

---

### Daily Backup Script

```bash
#!/bin/bash
# /usr/local/bin/backup.sh
BACKUP_DIR=/var/backups/myapp
DATE=$(date +%Y-%m-%d)
mkdir -p $BACKUP_DIR
tar -czf $BACKUP_DIR/backup-$DATE.tar.gz /var/www/myapp
find $BACKUP_DIR -name '*.tar.gz' -mtime +30 -delete   # delete backups older than 30 days
echo "Backup completed: $DATE" >> /var/log/backup.log

# Make executable and schedule:
chmod +x /usr/local/bin/backup.sh
# Add to crontab: 0 2 * * * /usr/local/bin/backup.sh
```

---

### Output & Debugging

```bash
# Redirect cron output to a log (cron sends to mail by default):
0 2 * * * /usr/bin/backup.sh >> /var/log/backup.log 2>&1

# Check crond is running:
systemctl status cron

# Watch cron syslog entries:
grep CRON /var/log/syslog | tail -20
journalctl -u cron -f
```

---

## 11. Zombie Processes

A zombie process has finished executing but still has an entry in the process table — it's waiting for its parent to read its exit status. Zombies hold no CPU/memory but do consume a PID slot. Too many can exhaust the PID table and prevent new processes from starting.

---

### Identifying Zombies

```bash
ps aux | grep 'Z'               # Z in the STAT column = zombie
ps aux | grep defunct           # Alternative label
top                             # 'zombie' count shown in the header line
```

---

### How to Kill Zombie Processes

```bash
# Step 1: Find the zombie and note its PID
ps aux | grep 'Z'

# Step 2: Find its parent PID (PPID)
ps -o pid,ppid,stat,comm | grep Z
# OR
cat /proc/<zombie_PID>/status | grep PPid

# Step 3: Kill the PARENT — this forces it to clean up
kill -SIGCHLD <parent_PID>      # Signal parent to reap children
# If parent ignores it:
kill -9 <parent_PID>            # Force kill parent (init/systemd will reap orphans)

# Note: You CANNOT kill a zombie with kill -9
# It's already dead — no running code to receive the signal.
# Killing the parent is the only fix.

# Verify:
ps aux | grep 'Z'
```

---

### Prevention in Scripts

```bash
#!/bin/bash
child_process &
PID=$!
wait $PID   # wait() reaps the child immediately on exit
```

> Persistent zombies with no fixable parent: reboot is the last resort.

---

## 12. Networking Commands

`ping`, `curl`, `wget` already covered in Section 6. This section adds the diagnostic and socket-inspection tools critical for cloud and server work.

---

### `netstat` — Network Statistics (Legacy)

```bash
netstat -tuln           # TCP/UDP listening ports, numeric addresses
netstat -tulnp          # Same + which process owns the port (needs sudo)
netstat -an             # All connections including ESTABLISHED
netstat -r              # Routing table

# Is port 80 open?
netstat -tulnp | grep ':80'
```

> On modern systems, `ss` has replaced `netstat`. Install with: `sudo apt install net-tools`

---

### `ss` — Socket Statistics (Modern Replacement)

```bash
ss -tuln                # Listening TCP/UDP ports
ss -tulnp               # Same + process info (needs sudo)
ss -s                   # Summary statistics
ss -t state established # All established TCP connections

# What's on a specific port:
ss -tulnp | grep ':22'  # Who's listening on SSH
ss -tulnp | grep ':80'  # Who's listening on HTTP

# Connections to a specific remote IP:
ss -t dst 10.0.0.5

# Count established connections:
ss -t state established | wc -l
```

---

### `traceroute` — Trace Network Path

Shows the path packets take to reach a host and latency at each hop. Critical for diagnosing connectivity issues between AWS regions, VPCs, or external services.

```bash
traceroute google.com           # Trace route to google.com
traceroute -n google.com        # Numeric only (no DNS, faster)
traceroute -m 15 google.com     # Limit to 15 hops max
tracepath google.com            # Alternative (may be available instead)

# Reading output:
# Each line = one hop (router)
# Three latency values per hop (three probes)
# * * * = hop not responding (firewall dropping ICMP)
```

---

### DNS & IP Utilities

```bash
# Install if needed: sudo apt install dnsutils net-tools

nslookup google.com     # Query DNS for a hostname
dig google.com          # Detailed DNS query
dig +short google.com   # Just the IP
dig -x 8.8.8.8          # Reverse DNS lookup

ip a                    # Show all network interfaces and IPs
ip r                    # Show routing table
hostname -I             # Print all local IPs
```

---

### Quick Reference

| Command | What it tells you |
|---|---|
| `ss -tulnp` | Open ports and which process owns them |
| `ss -t state established` | All active TCP connections |
| `netstat -r` | Routing table / default gateway |
| `traceroute <host>` | Network path + latency at each hop |
| `ping -c 4 <host>` | Basic reachability + round-trip latency |
| `curl -I <url>` | HTTP response headers / health check |
| `dig +short <host>` | Quick DNS lookup |
| `ip a` | All interfaces and their IPs |