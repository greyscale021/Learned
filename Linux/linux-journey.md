# My Linux Journey — 7 Days

## Day 1 — Navigation

*The terminal felt foreign at first. Just a blinking cursor.*

Today was about learning how to move (navigation). The filesystem in Linux is a tree — everything starts from `/` (root) and branches out. I learned that every location has two ways to describe it: an **absolute path** that starts from root (`/home/user/Documents`), and a **relative path** that starts from wherever you currently are (`Documents`).

The commands I lived in today:

```bash
pwd     # where am I?
ls -lah # what's here? (long format, hidden files, human readable sizes)
cd ..   # go back
cd ~    # go home
cd -    # go to where I just was
```

I also learned `pushd` and `popd` — like a bookmark stack for directories(works like git stashing). Push a location, go somewhere else, pop back to it. Clean.

Found out about `find`, `locate`, and `stat`. The difference: `find` searches in real time, `locate` uses a pre-built index (faster but needs `updatedb` to be current), and `stat` shows you everything about a single file — size, permissions, timestamps, ownership.

The important directories started making sense:
- `/etc` is where all the configuration lives
- `/var/log` is where the system writes its diary
- `/bin` and `/usr` are where programs live
- `/proc` is wild — it's a virtual filesystem that shows running processes as files

*First real moment: typed `ls /proc` and saw process IDs listed as folders. The OS felt transparent for the first time.*

---

## Day 2 — File management

*Today I stopped being a read-only user.*

File management. Creating, copying, moving, deleting.

`touch` creates empty files — or updates timestamps on existing ones. `mkdir -p` was a small revelation: create an entire nested path in one command.

```bash
mkdir -p projects/cloud/aws/notes   # creates all four levels at once
```

Noted one after deleteing my experimental directory: `rm -rf` Removes a directory and everything inside it, forcefully, no confirmation.

Learned the difference between `cp` and `mv` — copy leaves the original, move doesn't. And `mv` doubles as a rename tool.

For viewing files I now have a toolkit:
- `cat` for small files
- `less` for large ones (paginated, searchable)
- `head` and `tail` for the top and bottom
- `tail -f` to watch a file update in real time — powerful for logs

Using `grep` we can search directly from the terminal:

```bash
grep -R "error" /var/log/   # search for "error" across all log files
```

---

## Day 3 — Users

*Linux is a multi-user system. Permission management is mandatory.*

Every file in Linux has three sets of permissions — for the **owner**, the **group**, and **everyone else** (others). Each set has three slots: read (`r`), write (`w`), execute (`x`). A dash means the permission isn't granted.

Reading `ls -l` output clicked today:

```bash
-rwxr-xr-- 1 ismail devs 1234 Mar 1 script.sh
# -         → regular file
# rwx       → owner can read, write, execute
# r-x       → group can read and execute, not write
# r--       → others can only read
```

`chmod` has two modes. Numeric is fast once you memorize it:

```bash
# r=4, w=2, x=1
chmod 755 script.sh   # owner: rwx, group: rx, others: rx
chmod 600 secret.txt  # owner: rw, everyone else: nothing
```

Symbolic mode is more readable for specific changes:

```bash
chmod u+x script.sh    # add execute for owner only
chmod o-r secret.txt   # remove read from others
```

Then ownership. `chown` changes who owns a file. `who`, `users`, `groups`, `id` — commands that tell you about users currently on the system.

Creating users and groups:

```bash
useradd -m newuser          # creates user + their home directory
groupadd developers         # creates a group
usermod -aG developers ismail  # add ismail to developers group
```

`su` to switch between users. The difference between `su username` and `su - username` matters — the dash loads the full environment of that user, like actually logging in as them.

*Spent time breaking permissions on a test file. Made it unreadable, then unexecutable. Fixed it.*

---

## Day 4 — Processes
Learned the key distinction early: a **process** is a running instance of a program. A **service** is a background process managed by the system (via `systemd`). They're related but not the same.

`ps aux` became my go-to snapshot — shows every process running, who owns it, how much CPU and memory it's using. `top` shows the same thing but live, updating every few seconds. Like a task manager but in the terminal.

```bash
ps aux | grep sleep    # find a specific process by name
pgrep sleep            # just get the PID directly
```

Killing processes:

```bash
kill <PID>      # polite — asks the process to stop
kill -9 <PID>   # force — the OS terminates it immediately
pkill sleep     # kill by name instead of PID
```

Background and foreground jobs were confusing at first. `Ctrl+Z` suspends a process, `bg` sends it to the background, `fg` brings it back. `jobs` shows what's currently suspended or backgrounded in the session.

`nice` and `renice` for priority: higher nice value = lower priority. The OS gives less CPU time to high-nice processes. Useful when running something heavy that shouldn't affect other work.


---

## Day 5 — Services

*Services, logs, and the tools to understand why things break.*

`systemctl` is the command center for services. Everything a service can need:

```bash
systemctl status nginx    # is it running? any recent errors?
systemctl start nginx     # start it
systemctl stop nginx      # stop it
systemctl restart nginx   # restart (stop + start)
systemctl enable nginx    # start automatically on boot
systemctl disable nginx   # don't start on boot
```

Then `journalctl` — the real diagnostic tool. This is where the system writes everything that happens to services:

```bash
journalctl -u nginx           # all logs for nginx
journalctl -xe                # most recent logs with full context (use after a crash)
journalctl -f                 # follow logs in real time
journalctl --since "10 min ago"
```

The diagnostic flow I now follow when something breaks:
1. `systemctl status <service>` — quick snapshot, see if it's running, see last few log lines
2. `journalctl -xe` — full picture, find the actual error
3. `/var/log/` — check raw log files if needed

Also learned system monitoring today:
- `free -h` — RAM usage
- `df -h` — disk space per filesystem
- `du -sh <dir>` — size of a specific directory
- `uptime` — how long the system has been running, load average

*Deliberately stopped a service, read the logs, found the error, restarted it.*

---

## Day 6 — The Pipe

*Data flows. Commands aren't islands — they connect.*

Shell redirection and piping. This is what makes the terminal genuinely powerful.

Linux commands talk through three streams:
- `stdin` — input coming in
- `stdout` — normal output going out
- `stderr` — error output going out

Redirection sends these streams to files:

```bash
echo "hello" > file.txt    # overwrite
echo "hello" >> file.txt   # append
command 2> errors.txt      # redirect errors only
command &> all.txt         # redirect everything
```

The pipe `|` is the real magic — it connects the output of one command directly into the input of the next:

```bash
ps aux | grep python           # find python processes
cat /var/log/syslog | grep ssh # find SSH entries in logs
ls -lah | less                 # paginate a long directory listing
```

Control operators change when commands run:

```bash
mkdir logs && cd logs          # only cd if mkdir succeeded
cd logs || mkdir logs          # only mkdir if cd failed
apt update ; apt upgrade       # run both regardless
```

*Built a small pipeline: `ps aux | grep python | awk '{print $2}'` to extract just the PIDs of Python processes. Three commands, one line, exactly what I needed.*

---

## Day 7 — Remote Machines

*SSH. This is how the real world works. Every server you'll ever touch as a cloud engineer is accessed this way.*

Started with the concept. Password login sends a secret over the network on every connection. Key-based login never sends a secret — you prove identity through cryptography instead.

The key pair:
- **Private key** (`~/.ssh/id_rsa`) — stays on your machine. Never shared. Never leaves.
- **Public key** (`~/.ssh/id_rsa.pub`) — goes on the server. Safe to share. Useless without the private key.

Generated my first key pair:

```bash
ssh-keygen -t ed25519   # modern, recommended algorithm
```

Copied the public key to the server:

```bash
ssh-copy-id user@host   # appends public key to server's authorized_keys
```

The `~/.ssh/` directory structure became familiar:

```bash
~/.ssh/
├── id_rsa              # private key — chmod 600, only I can read
├── id_rsa.pub          # public key — safe to share
├── authorized_keys     # public keys of users allowed in (on the server)
├── known_hosts         # servers I've connected to before
└── config              # shortcuts for connections
```

Disabled password login in `/etc/ssh/sshd_config`:

```bash
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
```

Then understood the handshake: server encrypts a random challenge with my public key, sends it back. Only my private key can decrypt it. I send the proof. Server says access granted.

*Tested SSH to localhost. Generated a key, copied it, connected without a password. Then disabled password auth and confirmed I could still get in — only via key. That's the moment it stopped being theory.*

---

## What This Week Built

Seven days. No GUI. Just the terminal, the filesystem, and the OS.

By the end I could:
- Move through a Linux system confidently
- Create and manage files, users, groups, and permissions
- Run, monitor, and kill processes
- Start, stop, and diagnose services
- Read logs and understand why things break
- Chain commands together with pipes and redirection
- Connect to remote machines securely with SSH

I hope this is enough foundation.
---