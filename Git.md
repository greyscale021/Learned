# Git

<details>
  <summary><strong>Index</strong></summary>

1. [Starting with Local Repository](#1-starting-with-local-repository)
2. [Tracking Changes in Your Local Repo](#2-tracking-changes-in-your-local-repo)
3. [Advanced Features for Managing Changes](#3-advanced-features-for-managing-changes)
4. [Branching in Git](#4-branching-in-git)
5. [Remote repository](#5-remote-repository)
6. [Reverting or Resetting Changes](#6-reverting-or-resetting-changes)
7. [Git Log & History](#7-git-log--history)

</details>

<details>
  <summary><strong>Resources</strong></summary>

1. [GitHub tutorial by freeCodeCamp](https://youtu.be/mAFoROnOfHs?si=tp2vgE0ygFhb9Zp7)
2. [Cheatsheet](https://git-scm.com/cheat-sheet)
3. [Git cheat sheet PDF](https://git-scm.com/cheat-sheet.pdf)

</details>

<details>
  <summary><strong>Things to be clear</strong></summary>

1. This documentary is only useful after you have finished a basic tutorial of git.
1. Most Git commands act on your current branch by default.
2. Terms:
    - HEAD: The last local commit on your current branch.
    - Staging Area: What you’ve added with git add.
    - Working Directory: What’s currently in your editor.
    - Current branch: The branch you are currently in.
    - Tracked file: The files that were atleast once added or commited
    
</details>

## 1. Starting with Local Repository

This section covers the basic commands you’ll need to initialize your own local Git repository and set it up.

- **Set up your Git identity** (optional, but recommended before committing to keep track of who made each change).
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```

- **Initialize a new Git repository** in your project folder.
    ```bash
    git init
    ```

- **Check the status of your files** (shows modified/unstaged files).
    ```bash
    git status
    ```

## 2. Tracking Changes in Your Local Repo

Once you’ve set up your local repository, you can start tracking and saving your changes. These commands will allow you to see which files have been modified and save those changes locally.

- **View changes** (see which files have been modified).
    ```bash
    git status
    ```

- **Stage files** (prepare them for committing).

    - Stage **all changes**:
      ```bash
      git add --all # Stage all changes, including file deletions
      ```

    - Stage **specific file**:
      ```bash
      git add <file>  # Stage a specific file
      ```
    - Unstage **one file**:
      ```base
      git restore --staged <file-name>
      ```
    - Unstage **everything**:
      ```base
      git restore --staged .
      ```
- **Commit changes** with a descriptive message:
    ```bash
    git commit -m "Message"
    git commit -am "Message" # Add and Commit changes 
    ```

## 3. Advanced Features for Managing Changes

- **Diff** (Will show the difference):

    At any moment, every file in Git lives in three possible versions:
    - HEAD: Snapshot of the last commit on your current branch.
    - Staging Area: What you’ve added with git add.
    - Working Directory: What’s currently in your editor.
  ```bash
  git diff          # Working directory vs Staging area
  git diff --staged # Staging area vs HEAD
  git diff HEAD     # Working vs HEAD
  ```
    - .. (Compares left with right): Shows differences between left and right (from left’s perspective).
    ```bash
    git diff left..right # Left vs right
    git diff HEAD..origin/main # How to turn the local head into origin main/remote repo. HEAD vs origin/main
    ```

- **Stashing Changes**:
    - Stash saves **tracked local changes** (files that have  been added or committed at least once) 
    and resets all **tracked files** in the working directory and staging area to match `HEAD`, 
    allowing you to switch branches or work on something else.
    ```bash
    git stash   # Saves tracked changes and resets working directory and staging to match HEAD
    git stash pop   # Re-applies the changes from stash and deletes that stash
    git stash apply # Re-applies the changes from stash, but keeps the stash for later
    ```

- **Rebasing** (reapply changes from one branch onto another):
    - Useful for cleaning up commit history and integrating changes more smoothly:
    ```bash
    git rebase <branch-name>  
    # Take the current branch’s commits and replay them on top of <branch-name>.
    # main:       A — B — C
    # feature:    A — B — D — E
    # after "git rebase main"(feature): feature: A — B — C — D — E
    ```
    ```bash
    git rebase -i <branch-name>  # i stands for interactive, by using this we can edit(editing, squashing, reordering or deleting commits) the commit history.
    ```
    ```bash
    git pull --rebase # Fetches the latest commits from the remote branch and then reapply my local commits on top of them, instead of creating a merge commit.
    ```
- Good practice in pro workflow: Reason being when collaborating rebase can cause history confusion among different contriburtors.
    ```bash
    git fetch
    git diff HEAD..origin/main
    git stash            # If you have uncommitted work
    git pull --rebase
    git stash pop       
    ```
## 4. Branching in Git

Branching allows you to work on separate features or fixes in your codebase without affecting the main branch. This is crucial when working on multiple tasks simultaneously.

- **List all local branches**
    ```bash
    git branch
    ```

- **Create a new branch**:
    ```bash
    git branch <new-branch-name> # Creates a new branch
    git switch -c <new-branch-name> # Creates and change to the new branch
    ```

- **Switch to a different branch**:
    ```bash
    git switch <branch-name> 
    ```

- **Merge a branch into the current branch**:
    ```bash
    git merge <branch-name> # The current branch would merge with <branch-name> and update itself
    ```

## 5. Remote repository

We can connect our local repo to a Remote repository like GitHub, GitLab, etc. This allows a central hub to work from, or say collaborating with others.

- **Connect your local repository to a remote hub** (GitHub, GitLab, etc.):
    ```bash
    git remote add origin <repo-url>
    git branch --set-upstream-to=origin/main # It will set the current branch upstream to the origin/main without pushing
    ```
- Or, **Clone** (download) a remote repo to local:
    ```bash
    git clone <repo-url> # Downloads the repo to the current directory
    ```
- **Push** (Upload) your code to the remote repository (this uploads your changes as branch):
    ```bash
    git push -u origin <branch-name> 
    # Pushes your current local branch to remote as <branch-name>.
    # If the branch exists git will update it. If it doesn't exists git will create it.
    # After setting -u(upstream) with this flag, you can just use push or pull directly
    
    ```
    ```bash
    git push --all # Pushes all current local branches to the remote origin. Each branch will be created/updated as needed.
    ```
- **Fetch** (Retrieve changes from remote without changing your local branches):
    ```bash
    git fetch origin # Updates your local remote head(origin/main).
    ```
- **Pull** (Update) the latest changes from the remote repository (update your local repository with changes from the remote):
    ```bash
    git pull origin <branch-name>  # Pulls(fetch+merge) changes from remote <branch-name> to local current branch.
    ```
    ```bash
    git pull # If you already set upstream for the current branch.
    ```
    ```bash
    git pull --rebase # Fetches the latest commits from the remote branch, then re-applies the local commits on top of them, instead of creating a merge commit.
    ```

## 6. Reverting or Resetting Changes

Sometimes, you need to **undo** or **reset** changes either locally or even specific commits. This section deals with undoing mistakes and managing your commits effectively.

- **Restore**: Undo(reset to HEAD) local edits:
    ```bash
    git restore <file-name> # Undo ustaged changes of <file-name>.
    git restore . # Undo all ustaged changes
    ```
    ```bash
    git restore --staged <file-name> # Unstage staged changes of <file-name>
    git restore --staged . # Unstage all staged changes of <file-name>
    ```
    ```bash
    git restore --staged --worktree file.txt # Undo both working directory and staging changes of <file-name>
    git restore --staged --worktree . # Undo both working directory and staging changes.
    ```
- **Reset**: An old and versitile cmd.
    ```bash
    git reset <file> #Unstages <file> (staging → match HEAD)
    git reset . # Unstages all
    ```
    ```bash
    git reset --soft <commit> # Moves HEAD to <commit> but keeps staging & working dir
    ```
    ```bash
    git reset --mixed <commit> # Moves HEAD to <commit>, resets staging to match commit, working dir untouched
    ```
    ```bash
    git reset --hard <commit> # Moves HEAD to <commit>, resets staging & working dir to match commit
    ```
- **Revert**: Undo a commit safely by creating a new commit that reverses its changes.
    ```bash
    git revert <commit> # revert commit
    ```
## 7. Git Log & History

Viewing the history of commits allows you to track changes over time.

- **View commit history** (see all the previous commits in your repo):
    ```bash
    git log
    ```

- **View specific commits with details**:
    ```bash
    git log --oneline  # Shows commit IDs and messages in one line, sort order: newest to oldest
    ```
    ```bash
    git show <commit-hash> # Inspect one specific commit deeply.
    ```

- Move to commit:
    ```bash
    git switch --detach <commit-hash> # Will open in detached head stage.
    git checkout <commit-hash> # Old way
    ```
    ```bash
    git switch -c <new-branch-name> <commit-hash>
    ```