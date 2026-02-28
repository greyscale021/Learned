# Linux

<details>
    <summary><strong>Index</strong></summary>

1. [Linux navigation](#core-linux-navigation-commands)
2. [File management](#2-file-management-basics)
3. [Permissions & ownership](#3-permissions--ownership)
4. [Processes & System Control](#4-processes--system-control)
5. 
5. []()
5. []()
5. [Shell Redirection & Operators](#4shell-redirection--control-operators)
</details>

<details>
    <summary>Things to be clear</strong></summary>

1. The fundamental structure of a command in linux(unix like os) is:
`command [options] [arguments]`
 - Command: The name of the program to be executed (`ls`, `grep`, `cd`)
 - Options (Flags): Modifiers that change the behavior of the command, usually preceded by a dash (-) or two dashes (--) for spelled-out options
 - Arguments: The target of the command, such as a file name, directory, or text string
2. Combining Options: Single-character options can often be combined after a single dash (e.g., ls -la). 
3. sudo (superuser do) - makes you an admin for shortperiod, giving elevated privileges

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
ls -l   # Long format (permissions, owner, size, date)
ls -a   # Show hidden files (files starting with ".")
ls -la  # Show long formatted hidden files
ls -h   # Human-readable file sizes
ls -R   # Recursive (shows subdirectories)
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

---

## 2. File Management

These commands allow you to create, modify, copy, move, delete or view files and directories.

---
## Create/delete-
### `touch` (Create File)
```bash
touch <file> # Creates an empty file (if exists it will update the file)
```
### `mkdir` (Make directory)
```bash
mkdir <foldername>  # Creates a directory
mkdir <dir1> <dir2> # Creates multiple directories
mkdir -p a/b/c      # Creates nested directories | -p stands for parent
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
# -f for tails, stands for follow, it shows the realtime appended data in the last part of the file
```
### `grep` (Search for Text in Files)
```bash
grep "search_term" file.txt # Searches for search_term within file.txt
grep -R "something" somefile.txt    # Searches for something within somefile.txt
```
### `ln` (Link)
```bash
ln <somefile> <link-of-somefile> # Creates a hard link named link-of-somefile somefile
```
### `nano` (Simple text editor)
```bash
nano <file.txt> # Opens file in nano editor(Ctrl+o:save, Ctrl+x:exit)
```
---
## 3. Permissions & Ownership

In Linux, permissions control who(users) can read, write, and execute files. Each file has a set of permissions for the:

- User (u)(owner): the person who created the file
- Group (g): a group of users that are associated with the file
- Others (o): all other users

Each permission is represented by a letter:
- `r` (read: view contents of the file)
- `w` (write: modify the file)
- `x` (execute: run the file as a program)
- `-` (permission not granted)

## Seeing permissions-
### `ls -l` (long formatted list)
```bash
ls -l <file> 
# Example output: -rwxr-xr-- 1 user group 1234 Jan 1 file.txt
# first column: file type (regular-file: -, directory: d, link: l) 
# and permissions split into three groups (per three characters): owner(rwx) | group(r-x) | others(r--)
# second : number of hard link
# third : owner
# fourth : group
# fifth : file size
# sixth : date and time
# last : file name
```

## Changing permissions-
### `chmod` (change mode)
Numeric mode
```bash
chmod <permissions> <file>
# r:4, w:2, x:1 
# so rwx = 7, rw- = 6, r-- = 4
# example: chmod 755 file.txt  # User gets rwx, group gets rx, others get rx
```
Symbolic mode
```bash
chmod [who][operator][permissions] <file>
# who: u (user/owner), g (group), o (others), a(all)
# operator: + (add), - (remove), = (set exactly)
# example: chmod u+x, g=r file.txt # add execute permission to user, set read permission to group
```

## Changing ownership-
### `chown` (change owner)
```bash
chown <owner>:<group> <file> # change the owner and group of the file to <owner> and <group>
chown -R <owner>:<group> <directory> # Recursively change the owner and group of the directory to <owner> and <group>
```

## Creating new users and groups-
### `useradd`
```bash
useradd <username>  # Create a user
```
### `passwd`
```bash
passwd  # Set the password for the current user
passwd <username>   # Set the password for the <username>
passwd -S [username]    # displays if an account is locked or active
passwd -l [username]    # disables a password
passwd -u [username]    # re-enables a password
passwd -d [username]    # removes the password
```

### `groupadd`
```bash
groupadd <groupname>  # Create a group
usermod -aG <groupname> <username>  # Add user to an existing group
```
### `su` (Switch user)
```bash
su <username>  # Switch to another user (with login shell)
su - <username> # Switch to a specific user and load their full environment
su -c <cmd> <user> # Run a single command as another user
sudo -i -u <username>  # Switch to another user (without needing their password if you're root or have sudo permissions)
```

## 4. Processes & System Control
## Viewing Processes-
### `top`
```bash
top     # Opens a real-time system monitoring tool (like task-manager)
htop    # Enhanced version of top (required installation)
```
### `ps` (Processes)
```bash
ps      # Shows processes running in the current shell
ps aux  # Shows all running processes in full format
ps -ef  # Another variant to show processes with more details
ps -u <username>    # List processes for <username>
```

## Killing Processes-
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

## Process Control (Bandground/foreground)-
### `job` (Process that was started from the current session)
```bash
job # See all the current jobs and job Ids
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
### `shutdown`
```bash
shutdown -h now   # Shutdown immediately
shutdown -h +10   # Shutdown in 10 minutes
shutdown -r now   # Reboot immediately
shutdown -r 22:00 # Reboot at 10:00 PM
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

## System Monitoring-
### `df` (Disk free)
```bash
df -h      # Human-readable format for disk usage
df -T	   # Type of file system (ext4, xfs, tmpfs)
df .	   # Info of the disk containing the current folder
```
### `du` (Disk usage)
```bash
du -sh <dir>     # Show the total size of a directory
du -ah <dir>     # Show the size of all files and directories
```
### `free`
```bash
free -h      # Show RAM usage in human-readable format
```
### `iostat` (Input-output status)
```bash
iostat       # Show CPU and I/O statistics
```
### `uptime`
```bash
uptime       # Display uptime, load average, and number of users
```

## Managing Services
### `systemctl` (System control)
```bash
systemctl status <service_name>   # Check the status of a service

systemctl start <service_name>    # Start a service
systemctl stop <service_name>     # Stop a service
systemctl restart <service_name>  # Restart a service

systemctl enable <service_name>   # Enable service to start at boot
systemctl disable <service_name>  # Disable service from starting at boot
```

## Start process with priority
### `nice`
```bash
nice -n <priority> <command>  # Start a process with lower/higher priority
```
### `renice`
```bash
renice <priority> -p <PID>  # Change priority of an existing process
```

## File System Management for Processes
### `mount`
```bash
mount <directory>   # Mount a drive to <directory>
```
### `unmount`
```bash
umount <directory>   # Unmount the file system from <directory>
```



## 4.Shell Redirection & Control Operators


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

`|` (Redirect stdout → Command)
```bash
ps aux | grep python 
# Redirects the stdout of the left to the stdin of the right cmd.
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