# Git-Hub-Commads
This repo for all git hub commads using for all types This repository serves as your ultimate resource for mastering Git commands, offering a comprehensive collection tailored for all types of tasks. Discover the tools you need to elevate your version control skills!


# Git Command Reference & README

**Purpose:** A comprehensive, categorized list of essential Git commands and workflows — ideal for beginners to advanced users. Includes examples, conflict-resolution procedures, common error fixes, and recommended tools to level up your version-control skills.

---

## Table of Contents

1. Basics & Configuration
2. Staging & Committing
3. Branching & Inspecting
4. Pushing & Pulling (Remotes)
5. Merging & Rebasing
6. Resolving Conflicts (Step-by-step)
7. Working with Tags, Stash & Submodules
8. Undoing Changes: reset, revert, checkout
9. Inspecting History & Debugging (reflog, bisect)
10. Common Errors & How to Fix Them
11. Helpful Git Aliases
12. Recommended Tools & Integrations
13. README (Usage & Contribution)

---

## 1. Basics & Configuration

* `git init` — Create a new repository in the current folder.

  ```bash
  git init
  ```

* `git clone <repo-url>` — Clone a remote repository locally.

  ```bash
  git clone https://github.com/user/repo.git
  ```

* Configure identity (one-time):

  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

* View current settings:

  ```bash
  git config --list
  ```

* Set default editor (optional):

  ```bash
  git config --global core.editor "code --wait"
  ```

---

## 2. Staging & Committing

* Check status:

  ```bash
  git status
  ```

* Stage files:

  ```bash
  git add file.txt
  git add .           # stage all changes in cwd
  git add -p          # interactive patch staging
  ```

* Commit changes:

  ```bash
  git commit -m "Short concise message"
  git commit -am "Message"  # stage tracked changes and commit
  ```

* Amend last commit (when you forgot to include changes or want to update message):

  ```bash
  git commit --amend -m "Updated commit message"
  ```

* View commit log:

  ```bash
  git log --oneline --graph --decorate --all
  ```

---

## 3. Branching & Inspecting

* Create a branch:

  ```bash
  git branch feature/x
  ```

* List branches:

  ```bash
  git branch --all
  ```

* Switch/checkout branch:

  ```bash
  git checkout feature/x
  # or modern command
  git switch feature/x
  ```

* Create & switch in one step:

  ```bash
  git checkout -b feature/x
  # or
  git switch -c feature/x
  ```

* Delete a local branch:

  ```bash
  git branch -d feature/x   # safe (refuses if not merged)
  git branch -D feature/x   # force delete
  ```

* Rename a branch:

  ```bash
  git branch -m old-name new-name
  ```

---

## 4. Pushing & Pulling (Remotes)

* Add remote:

  ```bash
  git remote add origin https://github.com/user/repo.git
  ```

* List remotes:

  ```bash
  git remote -v
  ```

* Push a branch:

  ```bash
  git push origin main
  git push -u origin feature/x   # set upstream for future pushes
  ```

* Pull changes (fetch + merge):

  ```bash
  git pull origin main
  ```

* Fetch only (don't merge):

  ```bash
  git fetch origin
  ```

* Pull with rebase (clean linear history):

  ```bash
  git pull --rebase origin main
  ```

* Delete remote branch:

  ```bash
  git push origin --delete feature/x
  ```

---

## 5. Merging & Rebasing

* Merge a branch into current branch:

  ```bash
  git checkout main
  git merge feature/x
  ```

* Fast-forward vs. merge commit: If main hasn't moved, merge may fast-forward. Use `--no-ff` to force a merge commit:

  ```bash
  git merge --no-ff feature/x -m "Merge feature/x"
  ```

* Rebase a branch onto another (rewrite commits):

  ```bash
  git checkout feature/x
  git rebase main
  ```

* Interactive rebase (clean up commits):

  ```bash
  git rebase -i HEAD~5
  ```

* Abort a rebase:

  ```bash
  git rebase --abort
  ```

* Continue after resolving rebase conflicts:

  ```bash
  git add <fixed-files>
  git rebase --continue
  ```

---

## 6. Resolving Conflicts (Step-by-step)

1. When you run `git merge` or `git rebase` and encounter conflicts, Git will stop and mark conflicted files.
2. Use `git status` to list conflicted files.
3. Open each conflicted file and look for conflict markers:

   ```text
   <<<<<<< HEAD
   your changes
   =======
   incoming changes
   >>>>>>> branch-name
   ```
4. Edit the file to keep the desired code (remove markers).
5. Stage the resolved files:

   ```bash
   git add resolved-file.js
   ```
6. If merging: commit the merge:

   ```bash
   git commit                # Git created a merge message by default
   ```

   If rebasing: continue rebase:

   ```bash
   git rebase --continue
   ```
7. If you want to abort and go back to previous state:

   ```bash
   git merge --abort    # for merges
   git rebase --abort   # for rebases
   ```

**Helpful commands during conflicts**

* Show conflict markers quickly with `git diff`.
* Use `git mergetool` to launch an external merge tool configured in git config.
* Use `git checkout --theirs <file>` or `git checkout --ours <file>` to choose one side quickly, then `git add`.

Example to accept incoming branch's file:

```bash
git checkout --theirs path/to/file
git add path/to/file
```

Example to accept current branch's version:

```bash
git checkout --ours path/to/file
git add path/to/file
```

---

## 7. Working with Tags, Stash & Submodules

* Create lightweight tag:

  ```bash
  git tag v1.0
  ```

* Create annotated tag:

  ```bash
  git tag -a v1.0 -m "Release v1.0"
  ```

* Push tags:

  ```bash
  git push origin v1.0
  git push --tags   # push all tags
  ```

* Stash changes:

  ```bash
  git stash save "WIP: feature X"
  git stash       # stash with default message
  git stash list
  git stash apply stash@{0}  # apply without dropping
  git stash pop  # apply and drop
  git stash drop stash@{0}
  ```

* Submodules (when using another repo inside repo):

  ```bash
  git submodule add https://github.com/other/repo.git path/to/sub
  git submodule update --init --recursive
  ```

---

## 8. Undoing Changes: reset, revert, checkout

* Discard unstaged changes in working directory (dangerous):

  ```bash
  git checkout -- file.txt   # restore file from HEAD
  ```

* Unstage a file:

  ```bash
  git reset HEAD file.txt
  ```

* Soft reset (keep changes staged/uncommitted):

  ```bash
  git reset --soft HEAD~1
  ```

* Mixed reset (default) — unstage and keep working directory changes:

  ```bash
  git reset HEAD~1
  ```

* Hard reset (dangerous — discards commits & working dir changes):

  ```bash
  git reset --hard origin/main
  ```

* Revert a commit (safe, creates a new commit that undoes the change):

  ```bash
  git revert <commit-hash>
  ```

* Detached HEAD: If you checkout a commit directly, you're in a detached HEAD state. Create a branch if you want to keep work:

  ```bash
  git checkout -b my-branch
  ```

---

## 9. Inspecting History & Debugging

* Show commit ancestry graph:

  ```bash
  git log --oneline --graph --decorate
  ```

* Show changes between commits or branches:

  ```bash
  git diff main..feature/x
  git diff HEAD~1 HEAD
  ```

* Show a file content at a commit:

  ```bash
  git show <commit>:path/to/file
  ```

* Reflog — recover lost commits or see HEAD movements:

  ```bash
  git reflog
  git checkout <reflog-hash>
  ```

* Bisect — find a commit that introduced a bug by binary search:

  ```bash
  git bisect start
  git bisect bad       # current (bad) commit
  git bisect good v1.0 # known good
  # follow bisect instructions and mark each tested commit
  git bisect reset
  ```

---

## 10. Common Errors & How to Fix Them

### 1) `error: failed to push some refs` (non-fast-forward)

**Cause:** Remote has new commits you don't have.
**Fix:**

```bash
git pull --rebase origin main   # rebase your commits onto latest remote
# resolve conflicts if any, then
git push origin your-branch
```

Or merge:

```bash
git pull origin main
git push origin your-branch
```

### 2) Authentication failed (HTTP/SSH)

**Cause:** Wrong credentials, expired PAT, missing SSH key.
**Fix:**

* For HTTPS, configure credential helper or update stored credentials.
* If using GitHub, create/update a Personal Access Token and use it as password.
* For SSH, ensure `ssh-agent` has your private key and public key is added to GitHub/GitLab.

### 3) Merge conflicts

**Cause:** Concurrent edits in same file.
**Fix:** Follow the conflict-resolution steps in Section 6.

### 4) Detached HEAD

**Cause:** Checking out an arbitrary commit.
**Fix:** `git checkout -b new-branch` to preserve work.

### 5) `fatal: refusing to merge unrelated histories`

**Cause:** Histories have no common base (e.g., two separate inits).
**Fix:**

```bash
git pull origin branch --allow-unrelated-histories
```

But inspect carefully before using.

### 6) Corrupt object / repository errors

**Fix:**

```bash
git fsck --full
git gc --prune=now
# try clone fresh copy if repo is badly corrupted
```

### 7) Large files & push rejected (size)

**Fix:** Use Git LFS for large binary files:

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git commit -m "track large files"
```

---

## 11. Helpful Git Aliases

Add these to `~/.gitconfig` under `[alias]` for productivity:

```
[alias]
  st = status
  co = checkout
  br = branch
  cm = commit
  lg = log --oneline --graph --decorate --all
  last = log -1 HEAD
  unstage = reset HEAD --
```

---

## 12. Recommended Tools & Integrations

**CLI Tools**

* `gh` (GitHub CLI): manage PRs, issues, releases from terminal.
* `hub`: older GitHub helper CLI.
* `git-lfs`: for large files.

**GUI Clients**

* GitKraken — rich GUI (paid features).
* SourceTree — free GUI from Atlassian.
* GitHub Desktop — simple & free.
* Fork — macOS/Windows GUI.

**Editors & Extensions**

* Visual Studio Code + GitLens (history, code authorship) + built-in Git.
* JetBrains IDEs (IntelliJ, WebStorm) — excellent Git integration.

**Merge/Diff Tools**

* Meld (Linux), KDiff3, Beyond Compare, P4Merge — integrate with `git mergetool`.

**CI/CD & Hosting**

* GitHub Actions, GitLab CI, CircleCI, Travis CI.

**Learning / Visualization**

* `gitk` and `tig` (terminal UI) visualize history.
* Online: tryinteractive.github.io/learn-git or Git Immersion.

---

## 13. README (Usage & Contribution)

> Save this file as `README.md` in your repo root — it explains how to use this command reference and suggests how to contribute improvements.

---

# README

## Git Command Reference

This repository contains a compact and categorized reference of Git commands and workflows designed to help developers at all levels.

### Contents

* Basics & configuration
* Staging & committing
* Branching, merging, and rebasing
* Resolving conflicts
* Undoing mistakes and debugging
* Common errors and solutions
* Recommended tools to strengthen your version-control workflow

### How to Use

* Browse sections for the task you want to perform.
* Copy commands into your terminal, read the surrounding notes before running destructive commands (e.g., `--hard`).
* Use the aliases section to speed up daily Git usage.

### Contributing

If you spot outdated commands, missing workflows, or improvements:

1. Fork the repo.
2. Create a branch: `git checkout -b docs/git-updates`.
3. Edit `GIT_COMMANDS_AND_README.md` or `README.md`.
4. Commit your changes and push: `git commit -am "docs: improve git reference" && git push origin git-updates`.
5. Open a Pull Request describing your change.

### Testing Changes

* Non-code changes can be reviewed in PR and previewed on GitHub.

### License

Use this guide freely. Add a LICENSE file in your repo if you want to set permissions.

---

## Quick Tips & Best Practices

* Commit small, focused changes with clear messages.
* Use branches for features/bugfixes and protect `main`/`master` branches.
* Prefer `git pull --rebase` for local feature branches to keep a linear history.
* Use `git stash` when you need to switch context quickly.
* Regularly push to remote to avoid large divergence.

---

If you'd like, I can:

* Add a printable PDF version.
* Create a condensed cheat-sheet (1-page).
* Provide VS Code settings and recommended `git` config snippets.

Tell me which of those you'd like next.
