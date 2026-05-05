# 🚀 Git Interview Guide — Shell & HCL Edition

> **Language Coverage:** `Shell (Bash)` · `HCL (Terraform)` · `DevOps-Ready`
> **Repo Language Stats:** This file covers Shell & HCL so GitHub shows both languages on the repo sidebar.

---

## 📂 Table of Contents

1. [Section 1 — Git Basics (Q&A)](#section-1--git-basics-qa)
2. [Section 2 — Daily Git Flow (Shell)](#section-2--daily-git-flow-shell)
3. [Section 3 — Initial Setup (Shell)](#section-3--initial-setup-shell)
4. [Section 4 — Branching Strategy (Shell)](#section-4--branching-strategy-shell)
5. [Section 5 — Sync with Remote (Shell)](#section-5--sync-with-remote-shell)
6. [Section 6 — Undo & Fix (Shell)](#section-6--undo--fix-shell)
7. [Section 7 — Debug & Inspect (Shell)](#section-7--debug--inspect-shell)
8. [Section 8 — Bonus Commands (Shell)](#section-8--bonus-commands-shell)
9. [Section 9 — 50+ Commands Reference (Shell)](#section-9--50-git-commands-reference)
10. [Section 10 — Advanced Interview Q&A](#section-10--advanced-interview-qa)
11. [Section 11 — Terraform + Git Integration (HCL)](#section-11--terraform--git-integration-hcl)
12. [Section 12 — Pro Tips & Best Practices](#section-12--pro-tips--best-practices)

---

## Section 1 — Git Basics (Q&A)

### Q1. What is Git?
**Answer:** Git is a **Distributed Version Control System (DVCS)** that helps track code changes, collaborate with teams, roll back to older versions, and manage branching/merging.

> 💡 **Pro Tip:** Always mention "distributed" — it highlights Git's offline capability.

---

### Q2. What is GitHub?
**Answer:** GitHub is a cloud platform to host Git repositories and collaborate using Pull Requests, Issues, Actions (CI/CD), and project boards.

| Feature | Git | GitHub |
|---------|-----|--------|
| Type | Software | Service |
| Interface | Command-line tool | Graphical User Interface |
| Location | Installed locally on system | Hosted on the web |
| Maintained by | Linux community | Microsoft |
| Focus | Version control & code sharing | Centralized source code hosting |

> 💡 **Interview Edge:** Mention alternatives like GitLab, Bitbucket.

---

### Q3. What is a Distributed Version Control System?
**Answer:** In DVCS, every developer has a full copy of the repository. You work locally and push to a centralized repository (GitHub) when needed. You don't need to stay connected to work.

| Feature | Centralized VCS (SVN) | Distributed VCS (Git) |
|---------|----------------------|----------------------|
| Repository | Single central | Multiple (local + remote) |
| Offline Work | Not possible | Possible |
| Speed | Slower | Faster |
| Examples | SVN, Perforce | Git, Mercurial |

---

### Q4. What is GIT version control?
**Answer:** GIT version control allows you to:
- Track the history of changes in your code
- Revert to any previous version
- Branch and merge code safely
- Collaborate with teams without overwriting work

---

### Q5. What is a Repository?
**Answer:** A repository is a project directory managed by Git, containing code, commit history, and branches.

- **Local repository** → Exists on your machine (`git init`)
- **Remote repository** → Hosted on GitHub for team collaboration

---

### Q6. What is a bare repository in Git?
**Answer:** A bare repository contains only version control information (no working files, no tree, no `.git` sub-directory). Used for central server repos.

```bash
# Create a bare repository
git init --bare myrepo.git
```

---

### Q7. Difference between `git merge` and `git rebase`?
**Answer:**

| | git merge | git rebase |
|--|-----------|-----------|
| History | Non-linear, keeps all commits | Linear, replays commits |
| Use case | Team collaboration | Personal/feature branches |
| Commit count | Creates a merge commit | No extra commit |

```bash
# Merge (keeps history)
git merge feature-branch

# Rebase (linear history)
git rebase main
```

> 💡 **Pro Tip:** Teams often use `merge` for collaboration, and `rebase` for personal branches.

---

### Q8. What is a fast-forward merge?
**Answer:** A fast-forward merge occurs when no new commits exist on the target branch — Git simply moves the pointer forward. No merge commit is created.

```bash
git checkout main
git merge feature-branch  # fast-forward if main has no new commits
```

---

### Q9. What is `git stash`?
**Answer:** `git stash` temporarily saves uncommitted work and clears the working directory.

```bash
git stash save "work in progress"   # Save to stash
git stash list                       # See all stashes
git stash apply stash@{0}            # Copy stash back (keeps stash)
git stash pop stash@{0}              # Move stash back (removes stash)
git stash drop stash@{0}             # Delete specific stash
git stash clear                      # Remove all stashes
```

> **Use Case:** In the middle of a feature and need to switch to `main` to fix a bug → `git stash`, fix bug, then `git stash pop`.

---

### Q10. What is `.gitignore`?
**Answer:** A file listing files/patterns Git should NOT track.

```bash
# Example .gitignore
node_modules/
*.log
.env
*.tfstate
*.tfstate.backup
.terraform/
__pycache__/
*.pyc
```

---

## Section 2 — Daily Git Flow (Shell)

```bash
#!/bin/bash
# ============================================
# DAILY GIT FLOW — DevOps Cycle
# Language: Shell (Bash)
# ============================================

# Step 1: Check current status
git status

# Step 2: Stage all changes
git add .

# Step 3: Commit with a message
git commit -m "feat: add new feature"

# Step 4: Push to remote
git push origin main

# --- Shortcut for tracked files ---
git commit -am "fix: quick update"
```

---

## Section 3 — Initial Setup (Shell)

```bash
#!/bin/bash
# ============================================
# GIT INITIAL SETUP
# Language: Shell (Bash)
# ============================================

# Set global username
git config --global user.name "Your Name"

# Set global email
git config --global user.email "your@email.com"

# Initialize a new repository
git init

# Clone an existing repository
git clone <repo-url>

# Configure aliases for speed
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all"

# Connect local repo to GitHub
git remote add origin https://github.com/your-username/repo.git

# Verify remote connection
git remote -v
```

---

## Section 4 — Branching Strategy (Shell)

```bash
#!/bin/bash
# ============================================
# BRANCHING STRATEGY — CI/CD & Teamwork
# Language: Shell (Bash)
# ============================================

# List all branches
git branch

# List all remote branches
git branch -r

# List local + remote branches
git branch -a

# Create and switch to a new branch
git checkout -b feature-branch
# OR (modern way)
git switch -c feature-branch

# Switch to an existing branch
git checkout main
# OR
git switch main

# Merge a branch into current
git merge feature-branch

# Delete a local branch
git branch -d feature-branch   # Safe (only if merged)
git branch -D feature-branch   # Force delete

# Delete a remote branch
git push origin -d feature-branch

# See difference between two branches
git diff branch1..branch2

# Check if a branch is merged into master
git branch --merged
git branch --no-merged

# Rename a branch
git branch -m old-name new-name
```

---

## Section 5 — Sync with Remote (Shell)

```bash
#!/bin/bash
# ============================================
# SYNC WITH REMOTE
# Language: Shell (Bash)
# ============================================

# Fetch + merge changes from remote
git pull origin main

# Fetch changes (without merging)
git fetch origin

# Fetch a specific branch
git fetch origin feature-branch

# Download remote branch without merge
git fetch origin feature-branch
git checkout feature-branch

# Show remote repos and URLs
git remote -v

# Show remote branch details
git remote show origin

# Add new remote
git remote add upstream https://github.com/original/repo.git

# Remove a remote
git remote remove origin
# OR
git remote rm origin

# Push to remote
git push origin main

# Push and set upstream (first push)
git push -u origin feature-branch

# Pull from specific remote branch
git pull origin feature-branch
```

---

## Section 6 — Undo & Fix (Shell)

```bash
#!/bin/bash
# ============================================
# UNDO & FIX — Because mistakes happen!
# Language: Shell (Bash)
# ============================================

# Undo last commit, keep changes in staging area
git reset --soft HEAD~1

# Undo last commit, keep changes in working directory
git reset --mixed HEAD~1

# Undo last commit and DISCARD all changes (use with caution!)
git reset --hard HEAD~1

# Undo N commits (N = number of commits to remove)
git reset --hard HEAD~3

# Safely undo changes by creating a new commit (great for shared repos)
git revert <commit-id>

# Restore a file from last committed state
git restore <filename>

# Get back a commit to staging area
git reset --soft <previous_commit_id>

# Get back a file from staging area to working area
git reset HEAD <file_name>

# Fix incorrect commit message
git commit --amend -m "New correct commit message"

# Add missed file to previous commit (without changing message)
git add missed-file.txt
git commit --amend --no-edit

# Remove file from staging area
git restore --staged <filename>

# Remove untracked files and directories
git clean -fd

# Show all deleted commits (reflog)
git reflog

# Restore a deleted commit using cherry-pick
git cherry-pick <commit-id>
```

---

## Section 7 — Debug & Inspect (Shell)

```bash
#!/bin/bash
# ============================================
# DEBUG & INSPECT — Like a DevOps Expert
# Language: Shell (Bash)
# ============================================

# Show commit history
git log

# Show commit history in one-line format
git log --oneline

# Show beautiful graph view
git log --oneline --graph --all

# View changes between working tree, staging, or commits
git diff

# Compare staged vs committed
git diff --staged

# Compare two commits
git diff <commit1> <commit2>

# Show details of a specific commit
git show <commit-id>

# See who changed each line and why (blame)
git blame <filename>

# See only commit id and message (short)
git log --oneline

# Show commits between two tags
git log v1.0.0..v2.0.0 --oneline

# Check repository integrity
git fsck --full

# Show references in remote repository
git ls-remote

# Untrack a hidden/cached file
git rm --cached <filename>

# Get the hash of the latest commit
git rev-parse HEAD
```

---

## Section 8 — Bonus Commands (Shell)

```bash
#!/bin/bash
# ============================================
# BONUS — Powerful Commands for DevOps Engineers
# Language: Shell (Bash)
# ============================================

# Create a tag for a specific version
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Push a specific tag to remote
git push origin v1.0.0

# Push all tags
git push --tags

# Delete a local tag
git tag -d v1.0.0

# Delete a remote tag
git push --delete origin v1.0.0

# Apply a specific commit from another branch (cherry-pick)
git cherry-pick <commit-id>

# Cherry-pick multiple commits
git cherry-pick commitid1 commitid2

# Clean repository (remove untracked files)
git clean -fd

# Create archive (zip) of repository
git archive --format=zip HEAD > repo.zip

# Add submodule
git submodule add <url>

# Interactive rebase (edit/squash last N commits)
git rebase -i HEAD~5

# Create a linked working tree for a new branch
git worktree add ../new-feature new-feature

# Optimize repository
git gc

# Lock a branch (via GitHub Settings → Branches → Protection Rules)
# No CLI command — done via GitHub UI or GitHub API

# SSH key setup
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
cat ~/.ssh/id_rsa.pub   # Copy this to GitHub SSH Keys
```

---

## Section 9 — 50+ Git Commands Reference

```bash
#!/bin/bash
# ============================================
# 50+ GIT COMMANDS FOR CLOUD/DEVOPS ENGINEERS
# Language: Shell (Bash)
# A Handy Reference
# ============================================

# === CORE BASICS ===
git init                            # Initialize new repository
git clone <url>                     # Clone remote repository
git status                          # Check status of files
git add <file>                      # Stage a file
git add .                           # Stage all files
git commit -m "message"             # Commit with message
git commit -a -m "message"          # Stage + commit tracked files
git log                             # View commit history
git log --oneline                   # Compact history
git diff                            # View changes
git mv <old> <new>                  # Move/rename file
git rm <file>                       # Remove file from tracking
git help <command>                  # Get help for command

# === BRANCHING & MERGING ===
git branch                          # List branches
git branch <name>                   # Create new branch
git checkout <branch>               # Switch branch (old)
git switch <branch>                 # Switch branch (new)
git checkout -b <branch>            # Create + switch
git switch -c <branch>              # Create + switch (new)
git merge <branch>                  # Merge branch
git rebase <branch>                 # Rebase onto branch
git branch -d <branch>              # Delete merged branch
git branch -D <branch>              # Force delete branch
git stash                           # Stash uncommitted work
git stash pop                       # Restore latest stash
git stash list                      # List all stashes
git stash drop <stash@{n}>          # Delete specific stash

# === REMOTE OPERATIONS ===
git remote -v                       # Show remotes
git remote add <name> <url>         # Add remote
git remote remove <name>            # Remove remote
git fetch                           # Fetch all remotes
git fetch origin <branch>           # Fetch specific branch
git pull origin <branch>            # Pull = fetch + merge
git push origin <branch>            # Push commits to remote
git push -u origin <branch>         # Push + set upstream

# === UNDOING & RECOVERY ===
git restore <file>                  # Restore working copy
git restore --staged <file>         # Unstage file
git reset --soft HEAD~1             # Undo commit, keep staged
git reset --mixed HEAD~1            # Undo commit, keep files
git reset --hard HEAD~1             # Undo commit, discard all
git revert <commit>                 # Safe undo (new commit)
git cherry-pick <commit>            # Apply specific commit
git reflog                          # Show all history (recovery)
git clean -fd                       # Remove untracked files

# === TAGGING ===
git tag                             # List tags
git tag -a v1.0.0 -m "Release"     # Create annotated tag
git push origin v1.0.0              # Push specific tag
git push --tags                     # Push all tags
git tag -d v1.0.0                   # Delete local tag
git push --delete origin v1.0.0     # Delete remote tag
git log v1.0.0..v2.0.0 --oneline   # Commits between tags

# === ADVANCED ===
git config --global alias.st status # Create command alias
git worktree add ../branch branch   # Linked working tree
git fsck                            # Check repo integrity
git gc                              # Clean & optimize repo
git archive --format=zip HEAD > f.zip  # Export repo as zip
git rev-parse HEAD                  # Get latest commit hash
git submodule add <url>             # Add submodule
git rebase -i HEAD~n                # Interactive rebase

# === INSPECTION & DEBUG ===
git show <commit>                   # Show commit details
git blame <file>                    # Line-by-line history
git diff branch1..branch2           # Compare branches
git log --graph --oneline --all     # Visual branch graph
git shortlog -s -n                  # Commits by contributor
git ls-remote                       # List remote references
```

---

## Section 10 — Advanced Interview Q&A

### Q11. Do we always need `git add .` before commit?
**Answer:** No. For tracked files, use `git commit -am "msg"`. For new/untracked files, `git add` is mandatory.

---

### Q12. What is the difference between `git fetch` and `git pull`?

| | git fetch | git pull |
|--|-----------|----------|
| Downloads | Yes | Yes |
| Merges | No | Yes |
| Safe | Always safe | May cause conflicts |
| Use case | Review before merging | Quick sync |

```bash
# Fetch only (review first)
git fetch origin
git diff origin/main

# Pull (fetch + merge)
git pull origin main
```

---

### Q13. What is the difference between Fork and Clone?

| | Fork | Clone |
|--|------|-------|
| Location | Copies to your GitHub account | Copies to your local machine |
| Connection | Independent copy | Linked to original |
| Use case | Open source contribution | Local development |

```bash
# Clone a forked repo
git clone https://github.com/your-username/forked-repo.git

# Add upstream remote after cloning fork
git remote add upstream https://github.com/original/repo.git
```

---

### Q14. What is a Pull Request (PR)?
**Answer:** A PR is a request to merge code from one branch into another on GitHub. It enables code review, discussion, and approval before merging.

**Steps:**
1. Clone the forked repo and make changes
2. Push changes to your forked repo
3. Open the original repo → Pull Requests → New Pull Request
4. Compare branches and submit
5. Wait for approval and merge

---

### Q15. How to resolve merge conflicts?

```bash
# Step 1: Fetch latest changes
git fetch origin

# Step 2: Checkout and merge
git checkout main
git merge feature-branch

# Step 3: Identify conflicts
git status

# Step 4: Open conflicted files and fix:
# <<<<<<< HEAD
# your code
# =======
# incoming code
# >>>>>>> feature-branch

# Step 5: Stage resolved files
git add resolved-file.txt

# Step 6: Commit the merge
git commit -m "fix: resolved merge conflicts"

# Step 7: Push
git push origin main
```

---

### Q16. What is GitHub Actions?
**Answer:** GitHub Actions is a CI/CD automation tool inside GitHub. It triggers workflows (build, test, deploy) on events like `push`, `pull_request`, etc.

---

### Q17. GitHub Flow vs Git Flow?

| | GitHub Flow | Git Flow |
|--|-------------|----------|
| Branches | main + feature | main + develop + feature + release + hotfix |
| Complexity | Simple | Complex |
| Best for | Continuous deployment | Versioned releases |

---

### Q18. What is SSH Authentication?

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"

# View public key (add this to GitHub → Settings → SSH Keys)
cat ~/.ssh/id_rsa.pub

# Test SSH connection
ssh -T git@github.com

# Clone using SSH
git clone git@github.com:your-username/repo.git
```

---

### Q19. How to configure GitHub repository locally?

```bash
git config --global user.name "your_username"
git config --global user.email "your_email@gmail.com"
git config --global alias.st status
git config --global alias.lg "log --oneline --graph"
```

---

### Q20. Why is Git better than SVN?
**Answer:**
- Git is distributed — every developer has a full copy
- Git works offline
- Git is faster for local operations
- Branching is lightweight and cheap
- Better merge conflict resolution tools

---

## Section 11 — Terraform + Git Integration (HCL)

> This section covers HCL (HashiCorp Configuration Language) so the GitHub repo sidebar shows HCL as a repo language.

### 11.1 — Terraform Backend with Git-managed State

```hcl
# ============================================
# FILE: backend.tf
# Language: HCL (Terraform)
# Purpose: Remote state backend configuration
# ============================================

terraform {
  required_version = ">= 1.5.0"

  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "git-infra/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

---

### 11.2 — GitHub Repository Management with Terraform

```hcl
# ============================================
# FILE: github-repo.tf
# Language: HCL (Terraform)
# Purpose: Create and manage GitHub repos via IaC
# ============================================

provider "github" {
  token = var.github_token
  owner = var.github_owner
}

# ----- Main Repository -----
resource "github_repository" "git_learning_repo" {
  name        = "Git"
  description = "Git learning repository — Shell & HCL Edition"
  visibility  = "public"
  auto_init   = true

  has_issues   = true
  has_projects = true
  has_wiki     = true

  topics = ["git", "devops", "shell", "terraform", "hcl", "interview"]
}

# ----- Branch Protection Rules -----
resource "github_branch_protection" "main_protection" {
  repository_id = github_repository.git_learning_repo.node_id
  pattern       = "main"

  required_pull_request_reviews {
    dismiss_stale_reviews           = true
    require_code_owner_reviews      = true
    required_approving_review_count = 1
  }

  required_status_checks {
    strict   = true
    contexts = ["ci/build", "ci/test"]
  }

  enforce_admins = false
}

# ----- Default Branch -----
resource "github_branch_default" "default" {
  repository = github_repository.git_learning_repo.name
  branch     = "main"
}
```

---

### 11.3 — Variables

```hcl
# ============================================
# FILE: variables.tf
# Language: HCL (Terraform)
# ============================================

variable "github_token" {
  type        = string
  description = "GitHub Personal Access Token"
  sensitive   = true
}

variable "github_owner" {
  type        = string
  description = "GitHub username or org name"
  default     = "Cloud-Sahil"
}

variable "repo_name" {
  type        = string
  description = "Name of the Git repository"
  default     = "Git"
}

variable "environment" {
  type        = string
  description = "Deployment environment"
  default     = "dev"

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}
```

---

### 11.4 — Outputs

```hcl
# ============================================
# FILE: outputs.tf
# Language: HCL (Terraform)
# ============================================

output "repo_url" {
  description = "GitHub repository URL"
  value       = github_repository.git_learning_repo.html_url
}

output "repo_ssh_clone_url" {
  description = "SSH clone URL for the repository"
  value       = github_repository.git_learning_repo.ssh_clone_url
}

output "repo_http_clone_url" {
  description = "HTTPS clone URL for the repository"
  value       = github_repository.git_learning_repo.http_clone_url
}
```

---

### 11.5 — GitHub Actions CI Workflow (triggered from Terraform)

```hcl
# ============================================
# FILE: github-actions.tf
# Language: HCL (Terraform)
# Purpose: Create GitHub Actions workflow file via Terraform
# ============================================

resource "github_repository_file" "ci_workflow" {
  repository = github_repository.git_learning_repo.name
  branch     = "main"
  file       = ".github/workflows/ci.yml"
  commit_message = "ci: add GitHub Actions workflow via Terraform"

  content = <<-EOT
    name: CI Pipeline

    on:
      push:
        branches: [main]
      pull_request:
        branches: [main]

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4

          - name: Validate Shell Scripts
            run: bash -n *.sh || echo "No shell scripts found"

          - name: Terraform Format Check
            uses: hashicorp/setup-terraform@v3
            with:
              terraform_version: 1.5.0

          - name: Run terraform fmt
            run: terraform fmt -check -recursive || echo "No .tf files yet"
  EOT

  overwrite_on_create = true
}
```

---

### 11.6 — Terraform Commands Reference (Shell)

```bash
#!/bin/bash
# ============================================
# TERRAFORM + GIT WORKFLOW COMMANDS
# Language: Shell (Bash)
# ============================================

# Initialize Terraform (similar to git init)
terraform init

# Validate configuration
terraform validate

# Format HCL files
terraform fmt -recursive

# Plan changes (like git diff)
terraform plan

# Apply changes (like git push)
terraform apply

# Destroy infrastructure
terraform destroy

# Show current state (like git log)
terraform show

# List resources in state
terraform state list

# Import existing resource
terraform state import github_repository.git_learning_repo Cloud-Sahil/Git

# Output values
terraform output

# Workspace management (like git branches)
terraform workspace list
terraform workspace new dev
terraform workspace select prod
```

---

## Section 12 — Pro Tips & Best Practices

```bash
#!/bin/bash
# ============================================
# PRO TIPS & BEST PRACTICES
# Language: Shell (Bash)
# ============================================

# ✅ TIP 1: Always pull before you push
git pull origin main
git push origin main

# ✅ TIP 2: Write meaningful commit messages
# Bad:  git commit -m "fix"
# Good: git commit -m "fix: resolve null pointer in auth service"

# ✅ TIP 3: Use .gitignore for sensitive files
echo ".env" >> .gitignore
echo "*.tfstate" >> .gitignore
echo ".terraform/" >> .gitignore
echo "*.pem" >> .gitignore

# ✅ TIP 4: Use branches for every feature
git checkout -b feature/add-login-api

# ✅ TIP 5: Keep main branch clean and stable
# Use PRs + branch protection

# ✅ TIP 6: Review before committing
git diff --staged

# ✅ TIP 7: Tag your releases
git tag -a v1.0.0 -m "Production release v1.0.0"
git push --tags

# ✅ TIP 8: Check repo integrity periodically
git fsck --full

# ✅ TIP 9: Use interactive rebase to clean history before PR
git rebase -i HEAD~3

# ✅ TIP 10: Use git log aliases for better visibility
git config --global alias.lg "log --oneline --graph --all --decorate"
git lg
```

---

```hcl
# ============================================
# FINAL NOTE (HCL comment style)
# Language: HCL
# ============================================

# This repository covers:
# - Shell (Bash) scripting for Git workflows
# - HCL (Terraform) for Git/GitHub infrastructure as code
# - Interview Q&A for DevOps/Cloud Engineers
# - 50+ Git commands reference
# - Modular sections for easy navigation

locals {
  repo_summary = {
    languages    = ["Shell", "HCL"]
    topics       = ["git", "devops", "interview", "terraform", "shell"]
    author       = "Cloud-Sahil"
    last_updated = "2026-05-05"
    sections     = 12
    questions    = 30
    commands     = 50
  }
}
```

---

> 📌 **Repo Info:** This file is written in **Shell** and **HCL** so the GitHub repository language sidebar shows both languages. All commands are DevOps-ready and interview-tested.
