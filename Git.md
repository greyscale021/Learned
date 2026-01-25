# Git

<details>
  <summary><strong>Index</strong></summary>

1. [Starting with Local Repository](#1-starting-with-local-repository)
2. [Tracking Changes in Your Local Repo](#2-tracking-changes-in-your-local-repo)
3. [Advanced Features for Managing Changes](#3-advanced-features-for-managing-changes)
4. [Branching in Git](#4-branching-in-git)
5. [Connecting to a Remote Repository](#5-connecting-to-a-remote-repository)
6. [Collaboration: Local to Remote & Remote to Local](#6-collaboration-local-to-remote--remote-to-local)
7. [Reverting or Resetting Changes](#7-reverting-or-resetting-changes)
8. [Git Log & History](#8-git-log--history)

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
      git reset <file>
      ```
    - Unstage **everything**:
      ```base
      git reset
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
    - .. (Compares left with right)
    ```bash
    git diff HEAD..origin/main # What changed | how to make: Local latest commit to remote repo
    ```

- **Stashing Changes** (restores the working directory to match HEAD):
    - Save your local changes (Working directory and staged area's)  without committing them, allowing you to switch branches or work on something else:
    ```bash
    git stash   # Save your local changes temporarily
    git stash pop   # Retrieve the changes back after you’re done (apply then deletes stash)
    git stash apply # Apply only, stash stays
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
    git merge <branch-name> # With which the current branch would merge
    ```

## 5. Connecting to a Remote Repository

Now, let’s talk about how to link your local repository to a remote repository, like GitHub, GitLab, etc. This allows you to share your work with others or back it up.

- **Link your local repository to a remote** (GitHub, GitLab, etc.):
    ```bash
    git remote add origin <repo-url>
    ```

- **Push your code to the remote repository** (this uploads your changes as branch):
    ```bash
    git push -u origin <branch-name>
    # It will push your current local branch to remote as <branch-name>.
    # If exists git will update it. If doesn't exist git will create it.
    # The -u flag will make a connection then you don't need to use it again.
    
    ```
    ```bash
    git push --all # It will push all current local branches to the remote origin.
    ```

- **Pull the latest changes from the remote repository** (update your local repository with changes from the remote):
    ```bash
    git pull origin <branch-name>  # The remote branch you are pulling from.And it will land on the current local branch.
    or git pull                    # when you already set upstream with a remote branch.
    git pull --rebase              # It will pull and rebase with current branch.
    ```

## 6. Collaboration: Local to Remote & Remote to Local

Once your local repository is linked to a remote, you will work with others to push and pull changes.

- **Push your local changes to the remote**:
    ```bash
    git push origin <branch-name>
    ```

- **Fetch changes from the remote repository** (without merging them immediately):
    ```bash
    git fetch origin # Fetch: Updating your local remote head.
    ```

- **Pull the latest changes** (this downloads and merges remote changes into your local branch):
    ```bash
    git pull origin <branch-name>
    ```

## 7. Reverting or Resetting Changes

Sometimes, you need to **undo** or **reset** changes either locally or even specific commits. This section deals with undoing mistakes and managing your commits effectively.

- **Revert a file to the state of the last commit** (removes changes made locally):
    ```bash
    git restore <file>
    ```

- **Unstage a file** (removes the file from the staging area):
    ```bash
    git reset HEAD <file>
    ```

- **Revert a commit** (this creates a new commit that undoes a previous commit):
    ```bash
    git revert <commit-id>
    ```

- **Reset your working directory and index** (this can remove or unstage changes, or reset the commit history):
    ```bash
    git reset --hard <commit-id>   # Reset working directory and commit history
    git reset <commit-id>          # Soft reset, keeps changes in the working directory
    ```

## 8. Git Log & History

Viewing the history of commits allows you to track changes over time.

- **View commit history** (see all the previous commits in your repo):
    ```bash
    git log
    ```

- **View specific commits with details**:
    ```bash
    git log --oneline  # Shows commit IDs and messages in one line
    ```
