****#!/bin/bash****

# ============================================
# git-commands.sh
# Git Commands Reference Script for DevOps Engineers
# Author: Cloud-Sahil
# Language: Shell (Bash)
# ============================================

set -e

echo "======================================"
echo "  Git Commands Reference - Shell"
echo "======================================"

# ---- INITIAL SETUP ----
setup_git() {
  echo "[INFO] Configuring Git..."
  git config --global user.name "Your Name"
  git config --global user.email "your@email.com"
  git config --global alias.st status
  git config --global alias.co checkout
  git config --global alias.br branch
  git config --global alias.lg "log --oneline --graph --all --decorate"
  echo "[DONE] Git configured!"
}

# ---- INITIALIZE REPO ----
init_repo() {
  local dir="${1:-.}"
  echo "[INFO] Initializing Git in: $dir"
  cd "$dir"
  git init
  echo "[DONE] Repository initialized!"
}

# ---- DAILY FLOW ----
daily_flow() {
  echo "[INFO] Running daily Git flow..."
  git status
  git add .
  git commit -m "feat: daily update $(date +%Y-%m-%d)"
  git push origin main
  echo "[DONE] Daily flow complete!"
}

# ---- BRANCH MANAGEMENT ----
create_feature_branch() {
  local branch_name="${1:-feature/new-feature}"
  echo "[INFO] Creating branch: $branch_name"
  git checkout -b "$branch_name"
  echo "[DONE] Branch created and switched!"
}

merge_branch() {
  local branch="${1}"
  if [ -z "$branch" ]; then
    echo "[ERROR] Branch name required!"
    exit 1
  fi
  echo "[INFO] Merging $branch into current branch..."
  git merge "$branch"
  echo "[DONE] Branch merged!"
}

delete_branch() {
  local branch="${1}"
  echo "[INFO] Deleting branch: $branch"
  git branch -d "$branch"
  git push origin -d "$branch"
  echo "[DONE] Branch deleted locally and remotely!"
}

# ---- SYNC WITH REMOTE ----
sync_remote() {
  echo "[INFO] Syncing with remote..."
  git fetch origin
  git pull origin main
  echo "[DONE] Synced with remote!"
}

# ---- UNDO & FIX ----
undo_last_commit_soft() {
  echo "[INFO] Undoing last commit (keeping changes staged)..."
  git reset --soft HEAD~1
  echo "[DONE] Last commit undone!"
}

undo_last_commit_hard() {
  echo "[WARN] Undoing last commit and discarding all changes!"
  read -r -p "Are you sure? (yes/no): " confirm
  if [ "$confirm" = "yes" ]; then
    git reset --hard HEAD~1
    echo "[DONE] Hard reset done!"
  else
    echo "[INFO] Cancelled."
  fi
}

revert_commit() {
  local commit_id="${1}"
  if [ -z "$commit_id" ]; then
    echo "[ERROR] Commit ID required!"
    exit 1
  fi
  echo "[INFO] Reverting commit: $commit_id"
  git revert "$commit_id"
  echo "[DONE] Commit reverted safely!"
}

# ---- STASH OPERATIONS ----
stash_work() {
  local msg="${1:-WIP}"
  echo "[INFO] Stashing work: $msg"
  git stash save "$msg"
  echo "[DONE] Work stashed!"
}

restore_stash() {
  echo "[INFO] Restoring latest stash..."
  git stash pop
  echo "[DONE] Stash restored!"
}

list_stash() {
  echo "[INFO] Listing all stashes:"
  git stash list
}

# ---- DEBUG & INSPECT ----
inspect_history() {
  echo "[INFO] Commit History (graph view):"
  git log --oneline --graph --all --decorate
}

blame_file() {
  local file="${1}"
  if [ -z "$file" ]; then
    echo "[ERROR] File name required!"
    exit 1
  fi
  echo "[INFO] Blame for: $file"
  git blame "$file"
}

check_diff() {
  echo "[INFO] Staged changes:"
  git diff --staged
  echo "[INFO] Unstaged changes:"
  git diff
}

# ---- TAGGING ----
create_tag() {
  local version="${1:-v1.0.0}"
  local msg="${2:-Release $version}"
  echo "[INFO] Creating tag: $version"
  git tag -a "$version" -m "$msg"
  git push origin "$version"
  echo "[DONE] Tag created and pushed!"
}

delete_tag() {
  local version="${1}"
  if [ -z "$version" ]; then
    echo "[ERROR] Tag version required!"
    exit 1
  fi
  echo "[INFO] Deleting tag: $version"
  git tag -d "$version"
  git push --delete origin "$version"
  echo "[DONE] Tag deleted!"
}

# ---- CLEANUP ----
clean_repo() {
  echo "[INFO] Cleaning untracked files..."
  git clean -fd
  git gc
  echo "[DONE] Repository cleaned and optimized!"
}

# ---- SSH SETUP ----
setup_ssh() {
  local email="${1:-your@email.com}"
  echo "[INFO] Setting up SSH key for: $email"
  ssh-keygen -t rsa -b 4096 -C "$email"
  echo ""
  echo "[INFO] Your public key (add this to GitHub → Settings → SSH Keys):"
  cat ~/.ssh/id_rsa.pub
  echo ""
  echo "[INFO] Testing SSH connection to GitHub..."
  ssh -T git@github.com || true
}

# ---- GITIGNORE SETUP ----
setup_gitignore() {
  echo "[INFO] Creating .gitignore..."
  cat > .gitignore << 'EOF'
# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
*.swp
*.swo

# Node
node_modules/
npm-debug.log
yarn-error.log

# Python
__pycache__/
*.py[cod]
*.pyo
.env
venv/
*.egg-info/

# Terraform
.terraform/
*.tfstate
*.tfstate.backup
*.tfvars
.terraform.lock.hcl
crash.log

# Logs
*.log
logs/

# Secrets
*.pem
*.key
.env
.env.local
EOF
  echo "[DONE] .gitignore created!"
}

# ---- HELP MENU ----
help_menu() {
  echo ""
  echo "Usage: ./git-commands.sh <command> [args]"
  echo ""
  echo "Commands:"
  echo "  setup             - Configure Git globals"
  echo "  init [dir]        - Initialize repository"
  echo "  daily             - Run daily Git flow"
  echo "  branch <name>     - Create feature branch"
  echo "  merge <branch>    - Merge branch"
  echo "  delete <branch>   - Delete branch"
  echo "  sync              - Sync with remote"
  echo "  undo-soft         - Undo last commit (keep staged)"
  echo "  undo-hard         - Undo last commit (discard all)"
  echo "  revert <commit>   - Revert a commit safely"
  echo "  stash [msg]       - Stash current work"
  echo "  pop               - Restore latest stash"
  echo "  stash-list        - List all stashes"
  echo "  history           - Show commit graph"
  echo "  blame <file>      - Blame a file"
  echo "  diff              - Show staged/unstaged changes"
  echo "  tag <v> [msg]     - Create and push tag"
  echo "  delete-tag <v>    - Delete local + remote tag"
  echo "  clean             - Clean untracked files"
  echo "  ssh [email]       - Setup SSH key"
  echo "  gitignore         - Create .gitignore"
  echo "  help              - Show this menu"
  echo ""
}

# ---- MAIN DISPATCHER ----
main() {
  case "${1:-help}" in
    setup)        setup_git ;;
    init)         init_repo "${2}" ;;
    daily)        daily_flow ;;
    branch)       create_feature_branch "${2}" ;;
    merge)        merge_branch "${2}" ;;
    delete)       delete_branch "${2}" ;;
    sync)         sync_remote ;;
    undo-soft)    undo_last_commit_soft ;;
    undo-hard)    undo_last_commit_hard ;;
    revert)       revert_commit "${2}" ;;
    stash)        stash_work "${2}" ;;
    pop)          restore_stash ;;
    stash-list)   list_stash ;;
    history)      inspect_history ;;
    blame)        blame_file "${2}" ;;
    diff)         check_diff ;;
    tag)          create_tag "${2}" "${3}" ;;
    delete-tag)   delete_tag "${2}" ;;
    clean)        clean_repo ;;
    ssh)          setup_ssh "${2}" ;;
    gitignore)    setup_gitignore ;;
    help|*)       help_menu ;;
  esac
}

main "$@"
