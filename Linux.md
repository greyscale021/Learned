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
# displays content with <name> filter of <file>
grep -r "<name>" <directory>
# grep of folders
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
---
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
---
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

chmod -R <permissions> <dir>
# chmod of folder
# has to be capital r, because -r, acts as remove read
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
---
### Viewing ownership:
### `who`, `read` and `id`
```bash
users   
# displays logged in users name

who
# detailed version of users
# displays logged in users name+login_time

w
# detailed version of who
# displays logged in users name+login_time+activity
```
### `id` (compact who)
```bash
id
# display permissions and groups of all users
id <user>
# display permissions and groups of <user>

getent passwd
# display account configurations of all users
# shell, home dir, UID/GID
```
### `groups`
```bash
groups  
# displays all groups related to user

getent group
# displays all system groups

cat /etc/group  
# to see all group details
```
---
### Changing ownership:
### `chown` (change owner)
```bash
chown <user>:<group> <file> 
# change user and group of <file> to <user> and <group>

chown -R <user>:<group> <directory> 
# chown for folder

chown <user> <file>
# we can just change the user

chown :<group> <file>
# or just the group of the file
```
---

### Managing users and groups
### `useradd` (adds user)
```bash
useradd <name>  
# creates <name> user
useradd -m <name>
# creates <name> user with user directory

userdel <name> 
# deletes <name> user
userdel -r <name>
# deletes <name> user with user directory

adduser <name>
# adds all necessary flags to useradd
```
### `groupadd` (adds group)
```bash
groupadd <groupname>
# create <groupname> group
groupdel <groupname>
# deletes <groupname> group
```

### `passwd` (Password)
```bash
passwd
# set password for current user
passwd <username>
# set password for <username>
sudo passwd <username>
# to force change, bypass the minimu requiremnt

passwd -S <username>
# -S(status): displays password status
# P- password set, L- locked, NP- no password

passwd -l <username>
# -l(lock): change status to locked
passwd -u <username>
# -u(unlock): change status to password set
passwd -d <username>
# -d(del): change status to no password
```

### `su` (Switch user)
```bash
su <username>
# switch to <username>, keeps current env
su - <username>
# switch to <username>, changes env

su -c <cmd> <user> 
# run single command as <user>
sudo -i -u <username>
# switch to another user without password, with sudo
```

### `usermod` (Modify user)
```bash
usermod -g <group> <user>
# -g(primary group)
# change group of <user> to <group>

usermod -aG <group> <user>
# -a(append),-G(secondary group)
# add <user> to <group>

gpasswd -d <user> <group>
# removes <user> from <group>

usermod -L <user>
# lock <user>
usermod -U <user>
# unlock <user>
usermod -d /new/home <user>
# change home directory
```
---
## 4. Processes & System Control
### Process Control-
---
### `Monitoring`:
### `top` (task manager)
```bash
top
# opens monitoring tool
# q-quit
# k-kill process by pid(porcess id)
# shift + (p - sort by cpu, m - sort by memory, v - parent tree view)

htop
# enhanced version of top, requires installation
```
### `ps` (Processes)
```bash
ps      
# shows processes running in the current shell

ps aux
# a(all user),u(user oriented)
# x(include ps without terminal): include daemons

ps -ef  
# e(every process),f(full format)
# used in scripting
ps -u <username>    # List processes for <username>
```
```bash
pgrep <process> 
# PID grep of <process>
```
---
### `Priority`:
### `nice`
```bash
nice -n <priority> <command>  
# start a process with priority
# n value range(-20 to 19) defines priority
# lower n means higher priority 
```
### `renice`
```bash
renice <priority> -p <PID>  
# change priority of an existing process
```
---
### `Ending`:
### `kill`(kill by pid)
```bash
kill <PID>
# terminates <PID> proccess

kill -9 <PID>
# sigkill or force kill
```
### `pkill`(kill by name)
```bash
pkill <process-name>
# terminates <process-name> proccess
```
### `killall`(kill all by name)
```bash
killall <process-name> 
# terminates all <process-name> proccesses
```
---
### `Background/foreground`:
### `jobs`
The active background and suspended tasks managed by the current terminal session
 
```bash
jobs 
# displays all current jobs and job Ids
```
### `bg` (Background)
```bash
bg <job-id>     
# send process to background
# ctrl+z to suspend ps and display job number

bg %<job-id>
# resume ps in the background
```
### `fg` (Foreground)
```bash
fg <job_id> 
# bring job to foreground
```
---
### System Control-
---
### `Monitoring`:
### `uname`
```bash
uname -a
# displays all system information
```
### `uptime`
```bash
uptime
# displays uptime, load average, and number of users
```
### `iostat`(Input-output status)
```bash
iostat
# show CPU and I/O statistics
```
### `free`
```bash
free -h
# show RAM usage in human-readable format
```
### `df` (Disk free)
```bash
df
# shows mounted filesystems disk usage

df -h
# human-readable format
df -T
# type of file system (ext4, xfs, tmpfs)
df .
# Info of the disk containing the current folder
```
### `du` (Disk usage)
```bash
du <file_or_folder>
# shows specific files or folders disk usage

du -h <dir>
# human-readable
du -sh <dir>
# -s(summary),-h(human)
du -ah <dir>
# -a(all),-h(human)
```
Primary difference between df and du is, df uses filesystem metadata, and du scan files live
### `lsblk` (List block)
```bash
lsblk
# list info of all available block device
# hd,ssd,partitions etc
```
---

### `Managing`:
### `systemctl` (System control)
```bash
systemctl status <service_name>
# check the status of <service_name>

systemctl start <service>  # start <service>
systemctl stop <service>   # stop <service>
systemctl restart <service_name>  # restart <service>

systemctl enable <service_name>   
# enable service autostart
systemctl disable <service_name>  
# disable service autostart
```
### `journalctl` (diagnostic tool)
It's just better "tail -f /var/log/syslog"
```bash
journalctl -f
# -f(follow),displays real time logs
journalctl -f -n 4
# last 4 lines

journalctl -u <service>
# displays logs for a specific service
journalctl -u <service> -n 5
# last 5 lines

journalctl --since "<time>"
# <time>: "10 min ago","2026-03-07 10:30:00", "yesterday"

journalctl -xe
# -x(explanation),-e(jump to end)
# useful for troubleshooting crash

journalctl -p err
# displays only system errors
```

### `mount`
```bash
mount   
# list mounted file systems

mount <source> <target>
# mount <source> to <target>
```
### `umount` (Unmount)
```bash
umount <device_path>
# unmount file system of <device_path>
```
---
### `power`:
### `shutdown`
```bash
shutdown -h now
# -h(shutdown)
# shutdown immediately
shutdown -h +10
# shutdown in 10 minutes

shutdown -r now
# -r(reboot)
# reboot immediately
shutdown -r 22:00 
# reboot at 10:00 PM

shutdown -c       
# to cancel the schedule shutdown
```
### `reboot`
```bash
reboot
# reboot immediately
```
### `poweroff`
```bash
poweroff
# poweroff immediately
```
### `halt`
```bash
halt    
# os- stops, machine- running
```

## 5. SSH (Secure Shell)

SSH lets, securely connect and control remote machines(servers). Communications are encrypted.

---
**Login**:

**Password-based**: 

Login using password. Password travels through the network, though encrypted but risks of brute force and other types of attack.

**Key-based**: 

Login using key pairs. Private half of the key-pair is used by local machine, and public half is used by the server, ensures more safety.
- **Public key** (`id_ed25519.pub`): Public side of the keypair. (~username)
- **Private key** (`id_ed25519`): Private side of the keypair. (~password)

---
**Ssh handshake**:

Private key never leaves the client. Server makes a challenge with the public key, which can be solved only by the private key.

```
1. Client, tries to connect the server on port 22

2. Server, sends it's fingerprint
3. Client, checks known_hosts

4. Client, offers public key
5. Server, checks authorized_keys

6. Server, using public key, encrypts a challenge 
7. Client, decrypts it with private key

8. Server, confirms the result, the gives access
```
> metaphore: give a padlock to someone, which can be unlocked only by the key of the padlock.

---

**Directory Structure (~/.ssh/)**:

```bash
~/.ssh/
id_ed25519
# private key
id_ed25519.pub
# public key

authorized_keys
# server-side fingerprint of client keys
# keys authorized to log into server
known_hosts
# client-side fingerprint of servers
# client authenticating the server

# This two.. prevents man-in-the-middle 
# by bi-directional authentication
```

---
**Permissions**: 

SSH refuses keys if private key is too open.

```bash
chmod 700 ~/.ssh
# change permisison of .ssh to user(r,w,x)
chmod 600 ~/.ssh/id_ed25519     
# change permission of private key to user(r,w)
```

---

### Steps for ssh:
### 1. Server preparation

ssh: client tool and sshd: server daemon.

The server needs `sshd` running to accept connections.

From the server:
```bash
sudo apt install openssh-server -y
# install sshd service
sudo systemctl enable ssh   
# make ssh start on boot
sudo systemctl start ssh
# make ssh start now
sudo systemctl status ssh
# confirm status
```

### 2. Login(using password)

From the server:

Get the server's ip, username and check password authentication status(needs to be enabled).

```bash
ip a       
# to check ip

whoami     
# to check username

sudo nano /etc/ssh/sshd_config  
# to see "PasswordAuthentication"
```

From the client:

```bash
ssh username@ip
```

### 3. Set up key-based login

From the client:

Generate key pair:

```bash
ssh-keygen -t ed25519
# generates key of -t(type) ed25519
# ed25519 is the modern key type

ssh-keygen -t ed25519 -f /file_path
# -f(filename), generates key at /file_path
```

Copy public key to server:

```bash
ssh-copy-id -i ~/path user@ip
# ~/path of public key (.pub)
# copy public key to server
```

### 4. Securing server

For security reasons, key-based login is preferred.
After enabling key based login, disable password login for more security. 

From the server:

Disable password login and restart sshd.
```bash
sudo nano /etc/ssh/sshd_config
# change:
# PasswordAuthentication to 'no'
# PubkeyAuthentication to 'yes'
# PermitRootLogin to 'no'

sudo systemctl restart sshd
# restart sshd service
```

### 5. Login(using key-pair)

From the client:

```bash
ssh user@host
# keys will be used for authentiation

ssh username@ip "uptime"
# run single command in the server

ssh -p 2222 user@host
# connect on a specific port
# default port: 22

ctrl + D
# closes the connection
```

---
### Advanced: 
---
Skipping passphrasing:

`ssh-keygen` asks for a passphrase for keys. With passphrase the private key becomes much more secure. But we need to enter passphrase everytime we use the private key. We can skip it with ssh-agent.

```bash
eval "$(ssh-agent -s)"
# start ssh-agent

ssh-add ~/path
# ~/path of private key (keyname or .pem or .key)
# adds private key to ssh-agent
# ssh-agent will auto fill passphrase now
```
---
Server logs:

```bash
journalctl -u ssh -f
# live ssh logs of the server
```
---
File transfer:

We can transfer files using [scp](#file-transfer-). 
```bash
scp [target] [destination]

scp <file> user@host:/path
# copy local <file> to remote server

scp user@host:/path/file /path
# copy remote file to local /path

scp -r folder/ user@host:/path
# for folders, -r(recursive)

scp -P <port> <file> user@host:/path   
# -P(port)
# use <port> fort SSH, a non-default port
```
---

### Common Errors

| Error | Meaning | Fix |
|---|---|---|
| `permission denied (publickey)` | server doesn't have the public key | run `ssh-copy-id -i /public_key user@ip` |
| `WARNING: UNPROTECTED PRIVATE KEY FILE` | private key permissions too open | `chmod 600 /private_key` |
| `Host key verification failed` | server fingerprint changed | Verify server, update `known_hosts` |
| `Connection refused` | server problem | check `systemctl status ssh` on server |
| `Connection timed out` | firewall problem | check IP and firewall |

---

## 6. Package Management & File Transfer
A package manager manages softwares on linux (installing, updating, and removing). Different distros use different ones.

---
### Package Management-

---
### For debian based distro:

### `apt` (advanced package tool)

Searching:
```bash
apt search <package>
# search for <package>
apt show <package>
# show <package> details
```
Installing:
```bash
apt install <package>
# installs <package>
```
Updating:
```bash
apt update
# refresh package index 
# run before installation

apt upgrade
# update all installed packages
# uses package index reference

apt full-upgrade
# enhanced version of upgrade
# also can remove files if neccessary
```
Uninstalling:
```bash
apt remove <package>
# removes <package> 
# keeps config files

apt purge <package>
# removes <package>
# removes config files

apt autoremove
# remove unused dependencies
```
---
### For cross distro:
### `snap`
Snap packages are self-contained so they bundle their own dependencies for cross-distro usecase.

```bash
snap list
# list installed snaps

snap install <package>
# installs snap package

snap refresh
# update all snap packages

snap remove <package>
# removes snap package
```

---
### Archiving & Compression
---
 
Archiving: bundles files together <br> 
Compression: reduces their size

### `tar` (tape archive)
Used for archiving or compressing a directory.


```bash
tar -tf <archive>.tar
# -t(list-contents), -f(file-name)
# list contents without extracting

tar -cvf <archive>.tar <folder>
# -c(create), -v(verbose)
# create archive, with verbose(prints steps)

tar -xvf <archive>.tar
# -x(extract)
# extract an archive

tar -czvf <archive>.tar.gz <folder>
# -z(gzip)
# create gzip-compressed archive

tar -xzvf <archive>.tar.gz
# extract gzip-compressed archive
```

### `gzip`/`gunzip`
For compressing a file using GNU zip.

```bash
gzip <file>
# compress <file>

gzip -k <file>
# -k(keep)
# compress and also keep the original

gunzip <file>         
# extract compressed file
```
---
### Networking & File Transfer

---
### Networking-
### `ping`
Used for quickly checking network connectivity and latency.
```bash
ping <host>
# test if a host is reachable

ping -c <n> google.com    
# -c(count)
# send exactly <n> number packets

ping -i <n> google.com    
# -i(interval
# ping every <n> seconds
```
### `curl` (client URL)
Primarily a data transfer tool. Used to transfer data to or from a network server. Usually used for downloading stuffs.
```bash
curl <url>
# fetch content from a URL
# outputs to stdout

curl -o <file> <url> 
# -o(output)
# saves output as <file>

curl -I <url>   
# -I(inspect)(rensponse headers)
# fetch only the HTTP headers

curl -X POST -d "<data>" <url>
# -X(execute), -d(data)
# send a POST request with <data>
# POST method: used to upload data to a server

curl -H "Header-Name: Header-Value" <url>
# -H(header)(custom request headers)
# send request with header

curl -s <url>
# -s(silen)   
# silent mode, hides the download progress bar
```

### `wget` (web Get)
Primarily a file downloader. Built to fetch and crawl entire websites recursively.

```bash
wget <url>
# download file from <url>
wget -O <filename> <url>
# -O(output)
# download file from <url>, as <filename>

wget -c <url>
# -c(continue)
# resume interrupted download

wget -q <url>
# quiet mode

wget -r <url>
# recursive download (entire site)
```

> `wget` is better for downloading files. `curl` is better for API interaction.

---
### File transfer-

### `scp` (secure copy)
Secure copy, using [ssh](#5-ssh-secure-shell).

```bash
scp [target] [destination]

scp <file> user@host:/path
# copy local <file> to remote server
scp user@host:/path/file /path
# copy remote file to local /path

scp -r folder/ user@host:/path
# for folders, -r(recursive)

scp -P <port> <file> user@host:/path   
# -P(port)
# use <port> fort SSH, a non-default port
```

---

### `rsync` (remote Sync)
 rsync transfers pnly the changed files, making it more efficient than scp for repeated syncs.

```bash
rsync -av /path user@host:/path
# -a(archive), -v(verbose)
# sync local folder to remote
rsync -av user@host:/source /dest
# sync remote folder to local

rsync -av --delete /source /dest
# mirror, delete files not in source

rsync -avz /source user@host:/dest
# -z(compress)
# compress during transfer
```

## 7.Shell redirection & control operators


Terms to know:
- stdin  (standard input)
- stdout (standard output)
- stderr (standard error)

---
### Shell redirection

---
### Redirection Operators
Redirect output data (stdout,stderr) to file.

`>` (overwrite with stdout)
```bash
echo "hello" > file.txt
# overwrites file.txt, with stdout
```

`>>` (append with stdout)
```bash
echo "hello" >> file.txt
# appends to the end of the file, with stdout
```

`2>` (overwrite with stderr)
```bash
command 2> <file> 
# overwrites <file>, with stderr
```

`&>` (overwrite with stdout+stderr)
```bash
command &> <file> 
# overwrites <file>, with stdout+stderr
```
---
### Pipe Operator
Redirect output data (stdout) to stdin of command.

`|` (stdout to command stdin)
```bash
ps aux | grep "python"
# redirects stdout of first to the stdin of second
# pipe just takes stdout, and use it as stdin
```
---
### Substitution operator 
Uses stdout as command.

`$()` (stdout as command)
```bash
$(<command>)
# runs <command>, then the stdout
# is used as new entered command  
```

---
### Control operators
Execution flow :)

---
`&&` (AND)
```bash
command1 && command2
# run command1 and command2
# run the later one,only if earlier one succeeds
```

`||` (OR)
```bash
command1 || command2
# run command2, if command1 fails
# runs the later one,only if earlier one fails
```

`;` (Sequential)
```bash
command1 ; comman2  
# runs command sequentially
# regardless of success or failure
```
---

## 8. Text Processing

Three popular tools used for linux text processing. (grep, sed, awk)

---

### `grep` (filter)
```bash
grep "<name>" <file>
# displays content with <name> filter of <file>
grep -r "<name>" <directory>
# grep of folders
```


Also used for log work.

```bash
grep 'ERROR' <name.log>
# filters and prints
# lines containing ERROR of <name.log>

grep -r 'name' folder/
# recursive for directories

grep -i 'error' <name.log>
# -i(ignore case)
# search error case-insensitively

grep -n 'ERROR' <name.log>
# -n(number)
# show line numbers

grep -c 'ERROR' <name.log>
# -c(count)
# count matching lines

grep -B 2 'ERROR' app.log
# print 2 lines BEFORE match
grep -A 3 'ERROR' app.log
# print 3 lines AFTER match
```

---

### `sed`(Stream editor)

sed reads and applies editing commands line by line.Commonly used for substitution.

`Using substitution`: s/pattern/replacement/flags

Basic: edits the stream (stdout)
```bash
sed 's/old/new/' <file>
# s(substitution),old(loser),new(winner)
# replace first match per line and stdout

sed 's/old/new/g' <file>
# g(global)
# replace all matches per line
sed 's/old/new/gi' file.txt
# i(ignore case)
# case-insensitive global replace
```
In-place: edits the file directly, then stream (stdout)
```bash
sed -i 's/old/new/g' <file>         
# -i(in-place)
# saves changes fo <file>, then stout

sed -i.bak 's/old/new/g' <file>
# .bak (backup of <file>)
# make .bak then edit in-place
```
Other uses:
```bash
sed -n '5p' <file>
# n(number)
# prints line 5
sed -n '10,20p' <file>
# print lines 10-20
```
`Using address`: /pattern/action

Acts like an if statement. Applies to every line.

```bash
sed '/^#/d' <file>
# matches line, starting with '#'
# d(delete), the matching line.

# used for deleting comments

sed -E '/^(#|$)/d' <file>
# -E(ERE): using (e.g.:&,|) without "\"

# matches lines, starting with '#'
# or '$'(blank-line)
# d(delete), the matching line.

# used for deleting comments and blank lines
```
Example usecase:update config during provisioning
```bash
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