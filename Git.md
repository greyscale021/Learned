# Git

<details>
  <summary><strong>Learning Path</strong></summary>

**Tier 1 - Foundation**

1. [Starting with Local Repository](#1-starting-with-local-repository)
2. [Tracking Changes in Local Repo](#2-tracking-changes-in-local-repo)
3. [Branching in Git](#3-branching-in-git)
4. [Remote Repository](#4-remote-repository)
5. [.gitignore](#5-gitignore)

**Tier 2 - Daily Use**

6. [Reverting or Resetting Changes](#6-reverting-or-resetting-changes)
7. [Git Log & History](#7-git-log--history)
8. [Conventional Commits](#8-conventional-commits)
9. [Merge Conflicts](#9-merge-conflicts)
10. [Useful One-Liners & Aliases](#10-useful-one-liners--aliases)

**Tier 3 - Team & Production Workflows**

11. [Advanced Features - Diff, Stash, Rebase](#11-advanced-features--diff-stash-rebase)
12. [Tags & Releases](#12-tags--releases)
13. [Detached HEAD](#13-detached-head)
14. [Force Push Recovery](#14-force-push-recovery)

</details>

<details>
  <summary><strong>Resources</strong></summary>

1. [GitHub tutorial by freeCodeCamp](https://youtu.be/mAFoROnOfHs?si=tp2vgE0ygFhb9Zp7)
2. [Official Git Cheatsheet](https://git-scm.com/cheat-sheet)
3. [Git Cheat Sheet PDF](https://git-scm.com/cheat-sheet.pdf)

</details>

<details>
  <summary><strong>Things to be clear on before starting</strong></summary>

1. This document assumes you've already done a basic Git tutorial.
2. Most Git commands act on your **current branch** by default.
3. Key terms:
    - **HEAD** - Points to the last commit on your current branch. Think of it as "where you are right now."
    - **Working Directory** - What's currently in your file system / editor, including unsaved or unstaged edits.
    - **Staging Area (Index)** - A middle zone between your editor and a commit. Files go here with `git add` before being committed.
    - **Origin** - The default name for a remote repository.
    - **Tracked file** - A file Git is aware of (was added or committed at least once). Untracked files are invisible to Git.
    - **Upstream** - The remote branch your local branch is linked to, used by `push` and `pull` by default.

</details>

---

## 1. Starting with Local Repository

Set up a local Git repository from scratch, or configure your identity before making commits.

- **Set up Git identity** - Git tags every commit with your name and email. Set this once globally.
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```

- **Initialize a new repository** in your project folder.
    ```bash
    git init
    ```

- **Check repository status** - shows what's staged, unstaged, and untracked.
    ```bash
    git status
    ```

---

## 2. Tracking Changes in Local Repo

Once a repo is initialized, you track changes in a three-step cycle: **edit → stage → commit**.

- **Stage files** - Move changes into the staging area, ready to be committed.
    ```bash
    git add <file>        # Stage a specific file
    git add .             # Stage everything in current directory and subdirectories
    git add --all         # Stage all changes including deletions, anywhere in the repo
    ```

- **Unstage files** - Remove from staging area without discarding your edits.
    ```bash
    git restore --staged <file>   # Unstage a specific file
    git restore --staged .        # Unstage everything
    ```
> Tip - If you hadn't commited yet, it's your first add, to ustage use "git reset &lt;file&gt;"

- **Commit changes** - Save a snapshot of the staged files to history.
    ```bash
    git commit -m "your message here" -m "Description here"     # Commit with title and description.
    git commit -am "message"      # stage all and commit in one step. Only stage tracked file, does not stage new/untracked files      
    git commit      # For using a editor to write  title and desctiption.
    ```

> **Tip - Write good commit messages.** Use the imperative form: `"Add login route"` not `"Added login route"`. One line for small changes; use a body for complex ones. Good history makes debugging much easier later.

- **Untrack files**
    ```bash
    git rm --cached <file>      # Remove <file> from tracking file list.
    ```
---

## 3. Branching in Git

Branches let you work on features, fixes, or experiments in isolation from the main codebase. In real teams, working directly on `main` is almost always off-limits.

```bash
git branch          # List all local branches
git branch -a       # List local + remote-tracking branches
git branch <branch-name>      # Create a new branch (doesn't switch to it)
git switch <branch-name>      # Switch to a branch
git switch -c <branch-name>   # Create and switch in one step
git branch -d <branch-name>   # Delete a branch
git branch -D <branch-name>   # Force delete (even if unmerged)
```

- **Remote branch**
    ```bash
    git switch -c <branch> origin/<branch>    # Create a local <branch> from remote <branch> | A(remote) → A(local)
    git pull origin <branch>    # Fetches and merges <branch> from remote to current local branch | A(remote) → Current local branch
    ```

- **Merge a branch into the current branch:**
    ```bash
    git merge <branch>             # Merges <branch> into your current branch
    git merge --no-ff <branch>     # Force a merge commit even if fast-forward is possible. 
    # Preserves branch history, prefered in teams
    ```

> **Typical branch naming conventions in teams:**
> - `feat: add-login`
> - `fix: broken-redirect`
> - `chore: update-dependencies`
> - `release: v1.2.0`
> - `refactor: simplify-logic`

---

## 4. Remote Repository

Connect your local repo to a hosted remote (GitHub, GitLab, etc.) to back up code, collaborate, or trigger CI/CD pipelines.

- **Connect an existing local repo to a remote:**
    ```bash
    git remote add origin <repo-url>
    git remote -v                        # Verify the remote was added
    ```

- **Clone a remote repo to your machine:**
    ```bash
    git clone <repo-url>                 # Downloads repo into a new folder
    git clone <repo-url> <folder-name>   # Clone into a specific folder name
    ```

- **Push (upload) your changes:**
    ```bash
    git push -u origin <branch>   # Push branch + set upstream (do this once per branch)
    git push                      # Push to the tracked upstream (after -u is set)
    git push --all                # Push all local branches to remote
    git push --force-with-lease   # Force push safely - fails if remote has new commits you haven't seen
                                  # (safer than --force, use when rebasing a shared branch)
    ```

- **Fetch (download without merging):**
    ```bash
    git fetch origin              # Update remote-tracking refs (origin/main etc.) without touching local branches
    ```

- **Pull (fetch + merge or rebase):**
    ```bash
    git pull                      # Pull from tracked upstream
    git pull origin <branch>      # Pull specific remote branch into current local branch
    git pull --rebase             # Pull and rebase instead of merge (keeps history cleaner)
    ```

---

## 5. .gitignore

`.gitignore` tells Git which files to never track. This is **critical for security and cleanliness**, especially in cloud and DevOps work.

Create a `.gitignore` file in your project root:
```
# Dependencies
node_modules/
__pycache__/
*.pyc

# Environment variables (NEVER commit these)
.env
.env.*
*.env

# Cloud credentials (NEVER commit these)
.aws/credentials
*.pem
*.key
service-account.json

# Terraform (state files contain sensitive resource info)
*.tfstate
*.tfstate.backup
.terraform/
*.tfvars         # Often contains secrets

# Build outputs
dist/
build/
*.log

# OS files
.DS_Store
Thumbs.db
```

```bash
git rm --cached <file>
# Stop tracking a file that was already committed 
# Generaly after adding it to . gitignore
```
---

## 6. Reverting or Resetting Changes
`Restore`: resets to HEAD, `Reset`: move HEAD and `Revert`: new commit for commit fallback.

### restore - undo local edits
```bash
git restore <file>  # Discard unstaged changes of <file> (resets to HEAD)
git restore .   # Discard all unstaged changes
git restore --staged <file> # Unstage a staged file (staging area resets to HEAD)
git restore --staged --worktree <file>  # Undo both staged and unstaged changes
```

### reset - move HEAD
```bash
git reset <file>    # Unstage a file (same as restore --staged)

git reset --soft <commit>   # Move HEAD to <commit>, keep staging + working dir as-is
git reset --mixed <commit>  # Move HEAD to <commit>, clear staging, keep working dir  ← default
git reset --hard <commit>   # Move HEAD to <commit>, wipe staging + working dir
git reset --hard    # Nuke all uncommitted changes
```

### revert - undo a commit by a new commit
```bash
git revert <commit>     # Creates a new commit that undoes the changes made in <commit>
```

**When to use what:**

| Situation | Command |
|---|---|
| Undo unsaved local edits | `git restore` |
| Unstage something | `git restore --staged` or `git reset` |
| Undo a commit that's already on remote | `git revert` |

---

## 7. Git Log & History

```bash
git log  # Full commit history with author, date, message
git shortlog    # Show summary of commits grouped by author

git log --oneline   # Compact view: one commit per line (newest first)
git log --oneline --graph   # ASCII branch graph - useful for visualizing merges
git log --oneline --all     # Include all branches and remotes
git log --author="name"     # Filter commits by author
git log -- <file>           # Show commits that touched a specific file

git show <commit-hash>      # Full details of one specific commit
```

- **Move HEAD to a specific commit (detached HEAD mode):**
    ```bash
    git switch --detach <commit-hash>   # Explore history without being on a branch
    git checkout <commit-hash>          # Old equivalent
    ```

- **Create a branch from a specific commit:**
    ```bash
    git switch -c <new-branch> <commit-hash>
    ```

> **Tip:** `git log --oneline --graph --all` is one of the most useful commands for understanding your repo's state. Consider aliasing it. [(see section 10)](#10-useful-one-liners--aliases)

---

## 8. Conventional Commits

Conventional Commits is a widely adopted standard for writing commit messages in a structured, machine-readable way. Many CI/CD tools, changelogs, and release pipelines depend on it.

**Format:**
```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```
>**Note**: *For short description use imperative form.*

**Common types:**

| Type | When to use |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `chore` | Maintenance, dependencies, config (no production code change) |
| `docs` | Documentation only |
| `refactor` | Code restructure with no behavior change |
| `test` | Adding or fixing tests |
| `ci` | CI/CD pipeline changes |
| `perf` | Performance improvement |

**Examples:**
```
feat(auth): add JWT login endpoint
fix(api): handle null response from payment service
chore: update dependencies to latest versions
docs: add setup instructions to README
refactor(db): extract query builder into separate module
ci: add Docker build step to GitHub Actions
```

**Breaking changes** - add `!` after the type or a `BREAKING CHANGE:` footer:
```
feat!: remove deprecated /v1 API endpoints

BREAKING CHANGE: /v1 routes have been removed. Migrate to /v2.
```

> **Why this matters for cloud/DevOps:** Tools like semantic-release, GitHub Actions triggers, and auto-generated changelogs all parse commit types. A repo with consistent conventional commits is significantly easier to automate deployments around.

---

## 9. Merge Conflicts

A merge conflict happens when two branches edit the same part of the same file, and Git can't automatically decide which version to keep. Git pauses the merge and asks you to resolve it manually.

### When conflicts happen
- `git merge <branch>`
- `git pull` (which is fetch + merge)
- `git rebase`
- `git cherry-pick` (Pick a specific commit from a branch to current branch)

### What a conflict looks like in the file
```
<<<<<<< HEAD
  const port = 3000;
=======
  const port = 8080;
>>>>>>> feature/update-port
```
- Everything between `<<<<<<< HEAD` and `=======` is **your current branch's version**
- Everything between `=======` and `>>>>>>>` is **the incoming branch's version**

### How to resolve

1. Open the conflicted file and decide what the final version should be
2. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) and all unwanted code
3. Stage the resolved file:
    ```bash
    git add <file>
    ```
4. Complete the merge:
    ```bash
    git commit              # Finish a merge
    ```
    Or,
    ```bash
    git rebase --continue   # If the conflict happened during rebase
    ```

### Aborting if you want to back out
```bash
git merge --abort       # Cancel an in-progress merge, restore to pre-merge state
git rebase --abort      # Cancel an in-progress rebase
```

### Useful during conflict resolution
```bash
git status              # Shows which files are conflicted
git diff                # Shows remaining conflicts
git log --merge         # Shows commits from both branches that caused the conflict
```

> **Tip:** If conflicts happen often in the same files, that usually signals a structural problem - two people or features owning the same code. Worth discussing with the team.

---

## 10. Useful One-Liners & Aliases

**Helpful one-liners:**
```bash
git log --oneline --graph --all # Visual overview of all branches
git diff --stat HEAD            # Summary of changed files in compact view (not line-by-line)
git shortlog -sn                # Commit count per author
git stash list                  # View all stashes
git remote -v                   # Show remote URLs
git branch --merged             # Branches already merged into current
git branch --no-merged          # Branches not yet merged
```

**Set up aliases** (shortcuts for long commands):
```bash
git config --global alias.st status
git config --global alias.co switch
git config --global alias.lg "log --oneline --graph --all"
```

After setting aliases:
```bash
git st         # same as git status
git co main    # same as git switch main
git lg         # same as git log --oneline --graph --all
```

To see and edit all alias:
```bash
git config --global --edit  # It will open in default editor
```

---

## 11. Advanced Features - Diff, Stash, Rebase

### Viewing Diffs

At any moment, a file can exist in three states: the last commit (HEAD), the staging area, and your working directory. `git diff` compares any two of these.

```bash
git diff                    # Working directory vs Staging area
git diff --staged           # Staging area vs HEAD
git diff HEAD               # Working directory vs HEAD
git diff left..right        # Any two refs: branches, commits, tags
git diff HEAD..origin/main  # What the remote has that your local HEAD doesn't
```
>*Note: Diff by default targets the next stage(working dir vs staging, staging vs Head)*

>*Note: diff left..right means, what changes turn the left to right* 

### Stashing Changes

Stash lets you save work-in-progress without committing, so you can switch branch or context cleanly. It only saves **tracked** files (files that have been added or committed at least once).

```bash
git stash                 # Save tracked changes, reset working dir + staging to HEAD
git stash pop             # Re-apply the latest stash and delete it from stash list
git stash apply           # Re-apply the latest stash but keep it in the list
git stash list            # See all saved stashes
git stash drop stash@{n}  # Delete a specific stash entry
```

> **Note:** Untracked files are not stashed by default. Use `git stash -u` to include them.

### Rebasing

Rebase replays your commits on top of another branch, keeping history linear and clean. Preferred over merge in many teams for feature branches.

```bash
git rebase <branch>       # Replay current branch's commits on top of <branch>
git rebase -i <branch>    # Interactive rebase: edit, squash, reorder, or drop commits
git pull --rebase         # Fetch + rebase instead of fetch + merge
```

**Visual:**
```
Before rebase (on feature branch):
  main:    A - B - C
  feature: A - B - D - E

After: git rebase main (while on feature):
  feature: A - B - C - D - E
```

> ⚠️ **Never rebase commits that have already been pushed to a shared branch.** It rewrites history, which breaks everyone else's local copies. Rebase is safe on your own local/feature branches only.

### Clean Daily Workflow (Professional Pattern)

```bash
git fetch
git diff HEAD..origin/main     # See what's changed on remote before pulling
git stash                      # Stash local work if needed
git pull --rebase              # Pull cleanly without a merge commit
git stash pop                  # Re-apply local work on top
```

```bash
git clean -fd                  # Remove all untracked files and directories (irreversible)
```

---

## 12. Tags & Releases

Tags mark specific commits as significant - usually version releases. CI/CD pipelines often trigger deployments on tag pushes.

```bash
git tag                           # List all tags
git tag v1.0.0                    # Create a lightweight tag on current commit
git tag -a v1.0.0 -m "Release"    # Annotated tag (has author, date, message - preferred)
git push origin v1.0.0            # Push a specific tag to remote
git push origin --tags            # Push all tags to remote
git tag -d v1.0.0                   # Delete a local tag
git push origin --delete <tag-name> # Delete a remote tag
```

---

## 13. Detached HEAD

**Detached HEAD** means your `HEAD` is pointing directly to a commit instead of to a branch. You're not "on" any branch - changes you make here won't belong to anything unless you create a branch from this state.

### How you get into detached HEAD
```bash
git switch --detach <commit-hash>   # Intentionally explore a past commit
git checkout <commit-hash>          # Old equivalent
```

### What you can do in detached HEAD
- Look around, run the code, inspect files - completely safe
- Make experimental commits - they exist but belong to no branch

### How to get back safely
```bash
git switch <branch>    # Go back to <branch> - experimental commits will be garbage collected
```

### Save your work from detached HEAD
If you made commits in detached HEAD that you want to keep:
```bash
git switch -c <new-branch-name>     # Create a branch from current position - your commits are now saved
```

> **Rule of thumb:** Detached HEAD is fine for read-only exploration. The moment you want to make changes, create a branch first.

---

## 14. Force Push Recovery

Force pushing (`git push --force`) rewrites remote history. If done on a shared branch, it can overwrite other people's commits. Here's how to recover from common force push disasters.

### Prevention first
```bash
git push --force-with-lease    # Safer: fails if someone else pushed since your last fetch
                               # Use this instead of --force whenever possible
```

### Scenario 1 - You force pushed and want to undo it

```bash
git reflog  # Find the commit remote was on before your force push
git push origin <commit-hash>:main --force-with-lease  # Restore remote to that commit
```

### Scenario 2 - Someone force pushed and your local branch is now behind

```bash
git fetch origin
git log HEAD..origin/main       # See what's on remote that you don't have
git log origin/main..HEAD       # See what you have that remote doesn't

# Option A: Accept the remote version
git reset --hard origin/main

# Option B: Recover your commits and re-apply them
git rebase origin/main
```

### Scenario 3 - You reset --hard and lost commits

`git reflog` is your safety net. Git keeps a local log of every place HEAD has been, even after resets.

```bash
git reflog  # Find the commit hash from before the reset
git switch -c recovery-branch <commit-hash>     # Create a branch at that commit to recover your work
```

> **Reflog is local only** - it doesn't exist on the remote. This is why it can save you after a local `reset --hard`, but not after someone else force pushes and the remote history is gone.

### Quick reference - what to do when things go wrong

| Situation | Fix |
|---|---|
| Committed to wrong branch | `git reset --soft HEAD~1` → stash → switch branch → unstash → commit |
| Committed sensitive file | `git reset --soft HEAD~1` → add to `.gitignore` → recommit |
| Accidentally deleted a branch | `git reflog` → find last commit → `git switch -c <branch> <hash>` |
| Force pushed and broke remote | `git reflog` → find old commit → force push it back |
| Merge gone wrong | `git merge --abort` (if still in progress) or `git reset --hard ORIG_HEAD` |

---