# Linux

<details>
    <summary><strong>Index</strong></summary>

1. [Linux navigation](#1-core-linux-navigation-commands)
2. [File management](#2-file-management)
3. [Permissions & ownership](#3-permissions--ownership)
4. [Processes & System Control](#4-processes--system-control)
5. [SSH (Secure Shell)](#5-ssh-secure-shell)
6. [Package Management & File Transfer ](#6-package-management--file-transfer)
7. [Shell Redirection & Operators](#7shell-redirection--control-operators)
</details>

<details>
    <summary><strong>Things to be clear</strong></summary>

1. The fundamental structure of a command in linux(unix like os) is:
`command [options] [arguments]`
 - Command: The name of the program to be executed (`ls`, `grep`, `cd`)
 - Options (Flags): Modifiers that change the behavior of the command, usually preceded by a dash (-) or two dashes (--) for spelled-out options
 - Arguments: The target of the command, such as a file name, directory, or text string
2. Combining Options: Single-character options can often be combined after a single dash (e.g., ls -la). 
3. sudo (superuser do) - makes you an admin for shortperiod, giving elevated privileges
4. Process and Service are different things in linux
 - Process: A running instance of a program
 - Service: A background program managed by the system (usually by systemd)

</details>

<details>
    <summary><strong>Cheatsheet</strong></summary>
    
| Category | Key Commands |
|----------|-------------|
| Navigation | `pwd`, `ls`, `cd`, `tree`, `find`, `locate`, `stat`, `realpath`, `pushd`, `popd` |
| Files | `touch`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`, `ln`, `nano` |
| Viewing | `cat`, `less`, `head`, `tail`, `tail -f`, `grep` |
| Permissions | `chmod`, `chown`, `ls -l` |
| Users & Groups | `useradd`, `userdel`, `usermod`, `groupadd`, `gpasswd`, `su`, `passwd` |
| Ownership Info | `who`, `id`, `w`, `users`, `groups` |
| Processes | `ps`, `top`, `htop`, `pgrep`, `kill`, `pkill`, `killall`, `nice`, `renice` |
| Jobs | `jobs`, `bg`, `fg` — `Ctrl+Z` to suspend |
| System Info | `uname`, `uptime`, `free`, `df`, `du`, `lsblk`, `iostat` |
| Disk | `df`, `du`, `lsblk`, `mount`, `umount` |
| Services | `systemctl`, `journalctl` |
| SSH | `ssh`, `ssh-keygen`, `ssh-copy-id`, `scp`, `rsync` |
| Packages | `apt`, `snap` |
| Archiving | `tar`, `gzip`, `gunzip`, `zip`, `unzip` |
| Networking | `ping`, `curl`, `wget` |
| Shell Operators | `\|`, `>`, `>>`, `2>`, `&>`, `&&`, `\|\|`, `;` |
| Power | `shutdown`, `reboot`, `poweroff`, `halt` |
| Discovery | `man`, `whatis`, `which`, `whereis`, `type`, `history`, `alias` |
</details>

## 1. Core Linux Navigation Commands

These are the fundamental commands navigate in Linux.


###  `pwd` (Print Working Directory)

```bash
pwd # Shows your current location in the filesystem.
```


### `ls` (List)

```bash
ls      # Lists files in the current directory.
ls <folder> # Lists file in the <folder>
ls -l   # Long format (permissions, owner, size, date)
ls -a   # Show hidden files (files starting with ".")
ls -h   # Human-readable file sizes
ls -R   # Recursive (shows subdirectories)
ls -la  # Show long formatted hidden files
ls -lah # long format, hidden, human readable
```


### `cd` (Change directory)

```bash
cd <foldername> # Change directory to <foldername>
cd ..      # Go up/back one level
cd ~       # Go to home directory
cd /       # Go to root directory
cd -       # Go to previous directory
```

We can define paths as two types:

```bash
cd /home/user/Documents # Absolute Path (full path from root)
cd Documents            # Relative Path (from current directory)
```

### `tree` (if installed)

```bash
tree # Displays directory structure visually.
```


### `find`

```bash
find . -name "file.txt" # Search for files.
# Structure: find [starting-point] [expression]
# Starting-point: . current working directory, / root, ~ home.
```

### `locate`

```bash
locate filename # Fast file search (uses an indexed database).
sudo updatedb # to update the database for 'locate'
```

### `stat` (Status)

```bash
stat filename # Shows detailed file information(size, permissions, ownership, access)
```


### `realpath`

```bash
realpath file.txt # Shows the full absolute path of a file.
```


### `clear`

```bash
clear # Clears the terminal screen. Shortcut- (Ctrl + L)
```
### `pushd` (push directory) | `popd` (pop directory)
```bash
pushd <direcotry> # Moves to <direcotry> and saves previous directory onto a stack
popd # Pops the top directory from the stack and moves you there (LIFO)
dirs # Displays the current directory stack
```

---
## Command related tools :)

### `history` (command history)
```bash
history     # History of previous commands.
history <number>    # Show last <number> commands from recent
!<number>   # Run the <number> numbered command from history
!!      # Executes the last command
!<command>  # Executes the most recent command starting with <command>
history -c  # Clears the cmd history
```

### `whatis`
```bash
whatis <command> # Shows what does this command do in one sentence
```

### `type`
```bash
type <command> # Shows what kind of command is this
```

### `which`
```bash
which <command> # Shows the exact executable that will run when you type a command
```

### `whereis`
```bash
whereis <command> # Shows all known locations of a command
```

### `man` (Manual)
```bash
man <command> # opens the official manual for <command>
```
### `alias`
```bash
alias <new_name>='<command>'    # Makes a alias of <command> as <new_name>
unalias <name>      # Removes the alias
alias       # List of alias
```
---
### Important Directories

- `/home` → User directories  
- `/etc`  → System configuration files  
- `/var/log` → System logs  
- `/bin`  → Core command binaries  
- `/usr`  → Installed applications and libraries
- `/root` → Root user’s home
- `/tmp`  → Temporary files
- `/dev`  → Devices
- `/proc` → Running process info (virtual filesystem)
- `/opt`  → Optional software
- `/lib` → Essential shared libraries

### Reading logs in /var/log
```bash
tail -f /var/log/syslog        # watch live system logs
cat /var/log/syslog | grep ssh # filter logs for specific service
ls /var/log                    # see what log files exist
find /var/log -name "*.log"    # find all log files
find . -type f -mtime -1       # files modified in last 24hrs
```
---

## 2. File Management

These commands allow you to create, modify, copy, move, delete or view files and directories.

---
## Create/delete-
### `touch` (Create File)
```bash
touch <file> # Creates an empty file (if exists it will update the files timestamp)

touch -c <file.txt> # Change only the timestamp
touch -a <file.txt> # Change only the access time
touch -m <file.txt> # Change only the modification time
```
### `mkdir` (Make directory)
```bash
mkdir <foldername>  # Creates a directory
mkdir <dir1> <dir2> # Creates multiple directories
mkdir -p a/b/c  # Creates nested directories | -p stands for parent
```
### `rmdir` (Remove directory)
```bash
rmdir <foldername>  # Removes a directory (if empty)
```
### `rm` (Remove)
```bash
rm <file>           # Deletes a file
rm -R foldername    # Deletes a directory recursively (with everything in it)
rm -f filename.txt  # Force delete
rm -rf foldername   # Force delete directory recursively
```
## Copy/cut-
### `cp` (Copy)
```bash
cp <source> <destination>  # Copy from source to destination
cp -R folder1 folder2   # Copy directory recursively
```
### `mv` (Move / Rename)
```bash
mv <source> <destination>  # Move from source to destination
mv file.txt newname.txt # It can be used as renaming tool
```

## Archive-
### `zip`
```bash
zip <name.zip> <file>           # Zip <file> as <name.zip>
zip -r <name.zip> <directory>   # Zip <directory> as <name.zip>
```
### `unzip`
```bash
unzip archive.zip               # Extract zip archive
unzip -l archive.zip            # List contents without extracting
unzip archive.zip -d /path/     # Extract to a specific directory
```

## View-
### `cat` (Display / Concatenate)
```bash
cat <file>  # Display content
cat <file> <another-file>    # Concatenate two files as stdout
```
### `less` (View large files)
```bash
less <file> # View content of file with pagination(in pages)
```
### `head`
```bash
head <file> # View the first few lines of a file
```
### `tail`
```bash
tail <file> # View the last few lines of a file
tail -f <file>  # Follow file updates in real-time (for logs)
# -f (follow), it shows the realtime appended data in the last part of the file
```
### `grep` (Search for Text in Files)
```bash
grep "search_term" <file>  # Searches for search_term within the <file>
grep -R "search_term" <directory>  # Searches for something within directory
```
### `ln` (Link)
```bash
ln <file1> <file2> # Creates a hard link between file1 and file2 (both shares the same data and memory)
```
### `nano` (Simple text editor)
```bash
nano <file.txt> # Opens file in nano editor(Ctrl+O: Save, Ctrl+X: Exit)
```
---
## 3. Permissions & Ownership

In Linux, permissions control who(users) can read, write, and execute files. Each file has a set of permissions for the:

- User (u)(owner): The person who owns the file
- Group (g): Group of users that are associated with the file
- Others (o): All other users

Each permission is represented by a letter:
- `r` (read: view contents of the file)
- `w` (write: modify the file)
- `x` (execute: run the file as a program)
- `-` (permission not granted)
## Permission Control-
### `View`-
### `ls -l` (long formatted list)
```bash
ls -l <file> 
# Example output: -rwxr-xr-- 1 user group 1234 Jan 1 file.txt
# first column: file type (regular-file: -, directory: d, link: l) 
# permissions split into three groups (per three characters r,w,x): owner(rwx) | group(r-x) | others(r--)
# second : number of hard link
# third : owner
# fourth : group
# fifth : file size
# sixth : date and time
# last : file name
```

### `Change`-
### `chmod` (Change mode) -  Controls File/Directory Permissions
Numeric mode
```bash
chmod <permissions> <file>
chmod -R <permissions> <dir>
# r:4, w:2, x:1 
# rwx = 7, rw- = 6, r-x = 5, r-- = 4, --- = 0
# chmod 755 file.txt  (User- rwx, group- rx, others- rx)
```
Symbolic mode
```bash
chmod [who][operator][permissions] <file>
# who: u (user/owner), g (group), o (others), a(all)
# operator: + (add), - (remove), = (set exactly)
# chmod u+x, g=r file.txt :add execute permission to user, set read only permission to group
# chmod +x script.sh   (Add execute permission to all)
# chmod u+x,g+x,o+x script.sh (Previous command was equivalent to this)
```

## Ownership control-
### `View`-
### `who`
```bash
who     # Displays usernames, terminal lines, and login times
id      # User and group
w       # Shows logged-in users along with their current activity and idle time
```
### `users`
```bash
users   # All logged in users
getent passwd  # To see all users
id <user>      # To see all <user> details
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
# Higher the n value, lower the priority (default n value 0, lowest -20, highest 19)
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
# systemctl status <service>    #quick snapshot
# journalctl -xe        #full picture
# journalctl -u <service> -n 50     # Look into that service
# journalctl -b -1      # To look into boot
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

## 5. SSH (Secure Shell)

SSH lets you securely connect to and control a remote machine over a network. All communication is encrypted.
```bash
ssh user@host          # Connect to a remote machine
ssh user@ip            # Connect using IP address
ssh -p 2222 user@host  # Connect on a specific port (default is 22)
```
---

## Key Concepts

**Password login** — Type a password every time. The password travels over the network encrypted.

**Key-based login** — Generate a key pair. The private key stays on your machine. The public key goes on the server. No secret is transmitted — that's why it's safer.
- Public key (id_rsa.pub) — Safe to share, goes on the server
- Private key (id_rsa) — Never leave your machine

---

### `~/.ssh/` (Directory Structure)
```bash
~/.ssh/
|-- id_rsa              # your private key (chmod 600 — only you can read)
|-- id_rsa.pub          # your public key (safe to share)
|-- authorized_keys     # public keys allowed to log in (lives on the server)
|-- known_hosts         # servers you've connected to before (fingerprints)
 `- config              # optional: shortcuts for ssh connections

 # ssh = client, sshd = server daemon
```

Permissions matter — SSH will refuse to work if your private key is too open:
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
```

---

## Generating a Key Pair

### `ssh-keygen`
```bash
ssh-keygen                   # Generate with defaults (RSA 3072-bit)
ssh-keygen -t rsa -b 4096    # RSA 4096-bit (stronger)
ssh-keygen -t ed25519        # Ed25519 (modern, recommended)
```
Things-
```bash
# It will ask:
# Where to save (default: ~/.ssh/id_rsa)- press Enter to accept
# Passphrase- optional but adds an extra layer of protection

# After running ssh-keygen:
# ~/.ssh/id_rsa        # private key (auto-created)
# ~/.ssh/id_rsa.pub    # public key (auto-created)
```

---

## Copying Your Public Key to the Server

### `ssh-copy-id`
```bash
ssh-copy-id user@host                        # Copy your public key to server
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host   # Specify which key to copy

# It appends your public key to the server's ~/.ssh/authorized_keys
```

Manual equivalent:
```bash
cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

---

## Disabling Password Login

### `sshd_config`

Once key-based auth works, disable password login to lock the server down.
```bash
sudo nano /etc/ssh/sshd_config
```

Find and change these lines:
```bash
PasswordAuthentication no   # disable password login
PubkeyAuthentication yes    # ensure key auth is enabled
PermitRootLogin no          # never allow root to login directly
```

Apply the changes:
```bash
sudo systemctl restart sshd
```
---

## How the Handshake Works

`1.` Your machine connects to the server on port 22

`2.` Server sends its fingerprint — have you seen it before?
- Yes → auto-accepted from `known_hosts`
- No → you type `yes` → saved to `known_hosts`

`3.` Server checks `~/.ssh/authorized_keys` for your public key

`4.` Server generates a random challenge, encrypts it with your public key
- Only your private key can decrypt this

`5.` Your machine decrypts it with the private key, sends proof back

`6.` Server confirms — only the real private key could have done that → access granted

---

## 6. Package Management & File Transfer
A package manager handles installing, updating, and removing software on Linux. Different distros use different ones.
## Package Management-
### `apt` (Advanced pakage tool)
```bash
apt update              # Refresh the package index (always run before install)
apt upgrade             # Upgrade(update) all installed packages, using reference of pakage index
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

Archiving bundles files together. Compression reduces their size. `tar.gz` is standard on Linux servers. `zip` is useful for cross-platform sharing.

### `tar` (Tape Archive)
Flags-
- c - create
- x - extract
- v - verbose
- f - file
- z - gzip

```bash
tar -cvf archive.tar folder/        # Create an archive
tar -xvf archive.tar                # Extract an archive
tar -czvf archive.tar.gz folder/    # Create a gzip-compressed archive
tar -xzvf archive.tar.gz            # Extract a gzip-compressed archive
tar -tf archive.tar                 # List contents without extracting
```

### `gzip`/`gunzip`

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

```bash
rsync -av source/ user@host:/dest/      # Sync local folder to remote
rsync -av user@host:/source/ dest/      # Sync remote folder to local
rsync -av --delete source/ dest/        # Mirror (delete files not in source)
rsync -avz source/ user@host:/dest/     # Compress during transfer (-z)

# Flags:
# a - archive (preserves permissions, timestamps) | v - verbose | z - compress
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

`|` (Redirect stdout data → Command)
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