<details>
    <summary><strong>Index</strong></summary>

1. ## [Core linux naviagation commands](#core-linux-navigation-commands)
2. ## []()
</details>

## Core Linux Navigation Commands

These are the fundamental commands you must master to navigate Linux confidently.

---

### 1. `pwd` (Print Working Directory)

```bash
pwd # Shows your current location in the filesystem.
```
---

### 2️. `ls` (List)

```bash
ls      # Lists files in the current directory.
ls -l   # Long format (permissions, owner, size, date)
ls -a   # Show hidden files (files starting with `.`)
ls -la  # Show long formatted hidden files
ls -h   # Human-readable file sizes
ls -R   # Recursive (shows subdirectories)
ls -lah # long format, hidden, human readable
```

---

### 3️. `cd` (Change directory)

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
---


### 4. `tree` (if installed)

```bash
tree # Displays directory structure visually.
```

---

### 5. `find`

```bash
find . -name "file.txt" # Search for files.
# Structure: find [starting-point] [expression]
# Starting-pont: . current working directory, / root, ~ home.
```
---

### 6. `locate`

```bash
locate filename # Fast file search (uses an indexed database).
```
---

### 7. `stat` (Status)

```bash
stat filename # Shows detailed file information(size, permissions, ownership, access)
```
---

### 8. `realpath`

```bash
realpath file.txt # Shows the full absolute path of a file.
```
---

### 9. `clear`

```bash
clear #Clears the terminal screen. Shortcut- (Ctrl + L)
```

---

## Special Navigation Symbols

| Symbol | Meaning |
|--------|----------|
| `.`    | Current directory |
| `..`   | Parent directory |
| `~`    | Home directory |
| `/`    | Root directory |

 Examples:
```bash
cd ../.. # Go back two levels
cd ~/Downloads # Home directory/Downloads
./script.sh # Runs script from current directory
```
---
### Important Directories

- `/home` → User directories  
- `/etc` → System configuration files  
- `/var/log` → System logs  
- `/bin` → Core command binaries  
- `/usr` → Installed applications and libraries  

---