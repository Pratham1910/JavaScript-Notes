
# 🐙 Git & GitHub — Complete Tutorial (Basic → Advanced)

  

> **Git** is a version control system that tracks changes in your code. **GitHub** is a cloud-based platform that hosts Git repositories and enables collaboration. Together, they are the backbone of modern software development.

  

---

  

## 📚 Table of Contents

  

1. [The Big Picture: What is Version Control?](#1-the-big-picture-what-is-version-control)

2. [Setup & Configuration](#2-setup--configuration)

3. [The Basic Workflow (Add, Commit, Push)](#3-the-basic-workflow-add-commit-push)

4. [Branching & Merging](#4-branching--merging)

5. [Collaboration on GitHub (PRs & Forks)](#5-collaboration-on-github-prs--forks)

6. [Resolving Merge Conflicts](#6-resolving-merge-conflicts)

7. [Advanced: Git Stash](#7-advanced-git-stash)

8. [Advanced: The Power of `git reset` & `git revert`](#8-advanced-the-power-of-git-reset--git-revert)

9. [Advanced: Force Pushing & Amending](#9-advanced-force-pushing--amending)

10. [Working with Teams: The Flow](#10-working-with-teams-the-flow)

11. [What to Do (Compulsory)](#11-what-to-do-compulsory)

12. [What NOT to Do (Avoid)](#12-what-not-to-do-avoid)

13. [Quick Reference Cheat Sheet](#13-quick-reference-cheat-sheet)

  

---

  

## 1. The Big Picture: What is Version Control?

  

Imagine you are writing a book. You save "Version 1". You make changes and save "Version 2". Later, you realize Version 1 was better. Without version control, you've lost it.

  

**Git** keeps a history of every single change ever made. You can travel back in time, branch off to try new ideas, and merge those ideas back when they work.

  

---

  

## 2. Setup & Configuration

  

Before you start, tell Git who you are. This information is attached to your commits.

  

```bash

git config --global user.name "Your Name"

git config --global user.email "you@example.com"

```

  

### Starting a Repository

*   **Locally:** `git init` (Turns current folder into a Git repo).

*   **From GitHub:** `git clone https://github.com/user/repo.git` (Copies a remote repo to your computer).

  

---

  

## 3. The Basic Workflow (Add, Commit, Push)

  

### The Three Stages

1.  **Working Directory:** Files you are currently editing.

2.  **Staging Area (Index):** Files you have "marked" to be saved.

3.  **Local Repository:** The saved "snapshot" on your computer.

  

### The Commands

```bash

# 1. Check status

git status

  

# 2. Add files to Staging Area

git add index.html          # Add specific file

git add .                   # Add everything

  

# 3. Commit (Save locally)

git commit -m "Added a cool navigation bar"

  

# 4. Push (Upload to GitHub)

git push origin main

```

  

---

  

## 4. Branching & Merging

  

Branches allow you to work on new features without breaking the "Main" (production) code.

  

```bash

# Create a new branch

git branch feature-login

  

# Switch to that branch

git checkout feature-login

# OR (Short way to create and switch)

git checkout -b feature-login

  

# Merge feature-login back into main

git checkout main

git merge feature-login

```

  

---

  

## 5. Collaboration on GitHub (PRs & Forks)

  

When working with others, you don't usually push directly to `main`.

  

### Pull Requests (PRs)

1.  Push your feature branch to GitHub.

2.  Open a **Pull Request** on the GitHub website.

3.  Teammates review your code, leave comments, and eventually **Merge** it.

  

### Forking

If you don't have access to a repository, you **Fork** it (create your own copy on GitHub), make changes, and send a PR to the original owner.

  

---

  

## 6. Resolving Merge Conflicts

  

A conflict happens when two people change the **same line** in the **same file**. Git doesn't know which one to keep.

  

### How to Fix It:

1.  Git will mark the file:

    ```text

    <<<<<<< HEAD

    I like Blue

    =======

    I like Red

    >>>>>>> feature-branch

    ```

2.  Open the file and manually choose which line to keep (or combine them).

3.  Remove the `<<<<`, `====`, and `>>>>` markers.

4.  `git add <file>`

5.  `git commit -m "Resolved merge conflict"`

  

---

  

## 7. Advanced: Git Stash

  

**Scenario:** You are half-way through a feature, but you need to switch branches to fix an urgent bug. You aren't ready to commit yet.

  

**Solution:** `git stash`

  

```bash

# Temporarily "hide" your changes

git stash

  

# Check your stashes

git stash list

  

# Switch branches, fix the bug, come back...

  

# Bring your changes back

git stash pop

```

  

---

  

## 8. Advanced: The Power of `git reset` & `git revert`

  

### `git revert` (Safe)

Creates a NEW commit that does the exact opposite of a previous commit. **Use this on shared/remote branches.**

```bash

git revert <commit-hash>

```

  

### `git reset` (Dangerous)

Deletes commits from history as if they never happened. **Use ONLY on your own local branches.**

```bash

git reset --hard <commit-hash>  # DANGER: Deletes all uncommitted work!

```

  

---

  

## 9. Advanced: Force Pushing & Amending

  

### Amending a Commit

Forgot to add a file to your last commit? Use `--amend`.

```bash

git add forgotten-file.txt

git commit --amend --no-edit

```

  

### Force Pushing (`--force`)

If you've rewritten your history (like using `reset` or `amend`) and try to push, GitHub will reject it. You must force it.

  

**DANGER:** This can overwrite your teammate's work if they've pushed changes since your last pull.

  

```bash

# The "Safe" Force Push (Recommended)

git push --force-with-lease

```

*Note: `--force-with-lease` checks if someone else has pushed new code before overwriting.*

  

---

  

## 10. Working with Teams: The Flow

  

A standard team workflow:

1.  `git pull origin main` (Get latest code).

2.  `git checkout -b my-feature` (New branch).

3.  Make changes + `git add` + `git commit`.

4.  `git push origin my-feature`.

5.  Open Pull Request on GitHub.

6.  Address review comments.

7.  Merge into `main`.

  

---

  

## 11. What to Do (Compulsory)

  

*   ✅ **Commit Often:** Small, frequent commits are easier to manage than one giant commit.

*   ✅ **Write Good Messages:** `Fixed bug` is bad. `Fixed login timeout issue on mobile` is good.

*   ✅ **Pull Regularly:** Always pull before you start working to avoid conflicts.

*   ✅ **Use .gitignore:** Always include a `.gitignore` file to prevent sensitive files (like `.env` or `node_modules`) from being uploaded.

*   ✅ **Verify Before Committing:** Run `git diff` or `git status` to see what you are actually saving.

  

---

  

## 12. What NOT to Do (Avoid)

  

*   ❌ **NEVER commit secrets:** Do not push API keys, passwords, or `.env` files to GitHub.

*   ❌ **Don't force push to `main`**: This is the fastest way to break a project for the whole team.

*   ❌ **Don't use `git add .` blindly:** You might accidentally add a temporary file or a massive log file.

*   ❌ **Don't ignore conflicts:** Resolving them early is much easier than waiting a week.

*   ❌ **Don't share a single GitHub account**: Everyone should have their own.

  

---

  

## 13. Quick Reference Cheat Sheet

  

| Command | Action |

|---------|--------|

| `git status` | See what's happening. |

| `git log` | See commit history. |

| `git diff` | See line-by-line changes. |

| `git add <file>` | Stage a file. |

| `git commit -m "..."` | Save a snapshot. |

| `git push` | Upload to remote. |

| `git pull` | Download from remote. |

| `git stash` | Save current work for later. |

| `git checkout -b <name>` | Create and switch to branch. |

| `git merge <name>` | Combine code. |

| `git remote -v` | See remote URL. |

  

---

  

> **Summary:** Git is about **tracking changes** and GitHub is about **collaboration**. Master the basic workflow first, then move into stashing and conflict resolution as you join larger teams.

  

*Happy Coding! 🐙*