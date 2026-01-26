# git-github-daily-commands-for-sdet


# Git & GitHub Daily Workflow for SDET (From Scratch → Daily Work → Fixing Mistakes)

This repository documents **how Git and GitHub are actually used in real projects**, not theory.
It is written from an **SDET / Automation Engineer perspective**, with clear answers to:

* WHEN to use a command
* WHY it is needed
* WHAT problem it solves

If you understand this README, you can work confidently in any company project.


---


## 1. What Problem Git Solves (Reality)

In real projects:

* Multiple SDETs push automation daily
* Developers change code continuously
* You must track history, fix mistakes, and collaborate safely

Without Git:

* Code loss is guaranteed
* No rollback possible
* No accountability
* No team collaboration


---


## 2. Git vs GitHub (Clear Difference)

| Git             | GitHub               |
| --------------- | -------------------- |
| Tool            | Platform             |
| Runs locally    | Runs on cloud        |
| Tracks versions | Stores & shares code |
| Works offline   | Needs internet       |
| CLI based       | UI + collaboration   |

**Rule:**

* Git controls code
* GitHub connects people

---

### 2.1 Daily Git Workflow (Real Life)
```markdown
1. Pull latest code
2. Create branch
3. Do changes
4. Stage files
5. Commit
6. Push to remote
7. Create Pull Request
8. Review + Merge
```
---

### 2.2 Git Setup (One Time)
```bash
git config --global user.name "Manoj Kumar"
git config --global user.email "yourmail@gmail.com"
git config --list
```

---
-------------------------------------------------------------------------------
---


# SCENARIO 1: Create Project Locally and Push to GitHub (From Scratch)

This is the most common case for personal + learning projects.

---

## Preview end-to-end
```bash
git init
git status
git add .
git commit -m "comment: what you have be added"
git remote add origin <your repo url>
git push -u origin master
```


## Step 1: Create Project on Your Laptop

Example:

```
AutomationProject/
 ├── pom.xml
 ├── src/
 └── testng.xml
```

At this point:

* Git is NOT tracking
* GitHub does NOT know about this project


---

## Step 2: Initialize Git

```bash
git init
```

**WHY**

* Creates .git folder
* Starts version control
* Mandatory first step

**WHEN**

* Only once per project

---

## Step 3: Check File Status

```bash
git status
```

**WHY**

* Shows untracked / modified files
* Prevents accidental commits

**Professional habit:** Run before every commit

### Step 3.1: Check File Status

```bash
git log --oneline
```

---

## Step 4: Stage Files

```bash
git add .
```

**WHY**

* Selects files to save in history
* You control what goes into commit

---

## Step 5: Commit Changes

```bash
git commit -m "Initial project setup"
```

**WHY**

* Creates permanent snapshot
* Enables rollback anytime

**Rule:** One logical change = one commit

---

## Step 6: Create Repository on GitHub

GitHub → New Repository

**Important:**

* Do NOT add README
* Do NOT add .gitignore

Because your project already exists locally.

---

## Step 7: Connect Local Repo to GitHub

```bash
git remote add origin https://github.com/username/repo.git
```

**WHY**

* GitHub is a server
* Git needs destination address

---

## Step 8: Push Code to GitHub

```bash
git push -u origin main
```

**WHY -u**

* Links local branch to remote
* Future pushes need only `git push`

---
------------------------------------------------------------------------------------
---

# SCENARIO 2: Existing GitHub Repo → Daily Work (Company Flow)

This is daily life in real jobs.

---

## Preview end-to-end
```bash
git close <project repo url>
git checkout -b feature/LoginTest.java
write code or changes
git add .
git status
git commit -m "comment: what you have be added"
git push origin feature/logintest.java
from Github UI --> New PR(Pull Request) --> Compare branch --> Create & merge 
```


## Step 1: Clone Repository

```bash
git clone https://github.com/username/project.git
```

**WHY**

* Gets code locally
* Auto-connects remote

---

## Step 2: Create New Branch (MANDATORY)

```bash
git checkout -b feature/login-tests
```

**WHY**

* Never work on main
* Keeps code safe
* Enables review

---

## Step 3: Make Changes

Write tests, update framework, fix bugs.

---

## Step 4: Check Changes

```bash
git status
```

---

## Step 5: Stage Changes

```bash
git add .
```

---

## Step 6: Commit

```bash
git commit -m "Added login automation tests"
```

---

## Step 7: Push Branch

```bash
git push origin feature/login-tests
```

---

## Step 8: Create Pull Request (PR)

GitHub UI → New PR → Compare branch → Create

**WHY**

* Code review
* Team safety
* Mandatory in companies

---

## Step 9: Merge After Review

* Reviewer approves
* Merge to main
* Delete branch

---
----------------------------------------------------------------------------------------------------------
---

# Git Rebase (--rebase) – When, Where, Why (Professional Usage)

This section explains **exactly when and why `--rebase` is used in real projects**.
Do NOT use this blindly. Misuse breaks history.

---

## What Problem Rebase Solves

While you work on a feature branch:

* Other developers push changes to `main`
* Your branch becomes outdated
* Normal `git pull` creates extra merge commits
* Commit history becomes messy

Rebase solves this by keeping history clean and linear.

---

## What Rebase Actually Does

Rebase means:

> Re-apply my commits on top of the latest `main` branch

It rewrites commit history **locally**, not on remote.

---

## Where You Should Use `--rebase` (IMPORTANT)

Use rebase **only on your feature branch**:

```bash
git checkout feature/login-tests
git pull --rebase origin main
```

This updates your branch with latest `main` changes **without merge commits**.

---

## When You SHOULD Use Rebase

Use `--rebase` when:

* You are working on your own branch
* Branch is not shared with others
* You want clean commit history
* You want easy code review

---

## When You MUST NOT Use Rebase (Critical Rule)

Never rebase:

* `main` branch
* shared branches
* branches already pushed and used by team
* branches after PR review

Rebase rewrites history and will break others’ work.

---

## Correct Daily Flow with Rebase (Company Standard)

```bash
git checkout feature/login-tests
git pull --rebase origin main
# fix conflicts if any
git push origin feature/login-tests
```

Result:

* Clean history
* No merge commits
* Professional workflow
* Reviewer-friendly PR

---

## Handling Conflicts During Rebase

If conflict occurs:

```bash
# fix conflicts manually
git add .
git rebase --continue
```

If you want to cancel rebase:

```bash
git rebase --abort
```

This safely restores previous state.

---

## Simple Rule to Remember

* My branch only → rebase is safe
* Shared branch → NEVER rebase

---
---------------------------------------------------------------------------------------
---

# SCENARIO 3: Local Project + GitHub Repo Already Exists

This causes confusion. Use this clean fix:

```bash
git init
git remote add origin URL
git pull origin main --allow-unrelated-histories
git push -u origin main
```

**WHY**

* Syncs histories
* Avoids conflicts

---
---

# FIXING MISTAKES (REAL-LIFE SCENARIOS)

This is where most people panic. You should not.

---

## Mistake 1: Added Wrong File to Staging

```bash
git reset filename
```

**WHY**

* Removes from staging
* Keeps file unchanged

---

## Mistake 2: Wrong Commit Message

```bash
git commit --amend
```

**WHEN**

* Last commit only
* Not pushed yet

---

## Mistake 3: Want to Undo Last Commit (Keep Code)

```bash
git reset --soft HEAD~1
```

**WHY**

* Removes commit
* Keeps changes for re-commit

---

## Mistake 4: Want to Remove Last Commit (Delete Code)

```bash
git reset --hard HEAD~1
```

**WARNING**

* Code is permanently deleted
* Use carefully

---

## Mistake 5: Want to Discard Local Changes

```bash
git checkout -- filename
```

---

## Mistake 6: Conflict During Pull

```bash
git pull
# fix conflicts manually
git add .
git commit -m "Resolved merge conflict"
```

---

## Mistake 7: Pushed to Wrong Branch

```bash
git checkout correct-branch
git cherry-pick commit-id
```


---
#---
---

# DAILY PROFESSIONAL RULES (NON-NEGOTIABLE)

* Pull before starting work
* One task = one branch
* One commit = one logical change
* Never push to main directly
* Write meaningful commit messages
* Clean history matters
* README matters in interviews


---


## Final Truth

If you follow this README, you are already working at **company-level Git discipline**, not tutorial level.

This repo is designed for:

* SDETs
* Automation Engineers
* Interview preparation
* Real project confidence


---

**Author:** Manoj Kumar
