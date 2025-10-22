# Shell - Git Configuration

Git setup, configuration, and shell aliases for efficient version control workflows.

## Git Aliases

Add these convenient aliases to your shell configuration:

```bash
# Git
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log'
```

## Git Configuration

Set up your global Git configuration:

```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor vim

# Set default branch name for new repos
git config --global init.defaultBranch main

# Configure line endings
git config --global core.autocrlf input  # On macOS/Linux
# git config --global core.autocrlf true  # On Windows
```

## Global Git Configuration File

View your complete Git configuration at `~/.gitconfig`:

```ini
[user]
    name = Your Name
    email = your.email@example.com

[core]
    editor = vim
    autocrlf = input

[init]
    defaultBranch = main

[push]
    default = current

[pull]
    rebase = true

[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = restore --staged
    discard = restore
    last = log -1 HEAD
```

## Common Git Workflows

### Initialize a Repository

```bash
mkdir my-project
cd my-project
git init
```

### Clone a Repository

```bash
git clone git@github.com:USERNAME/repository.git
cd repository
```

### Create and Switch Branches

```bash
git checkout -b feature/my-feature
```

### Stage and Commit Changes

```bash
# Stage specific files
git add path/to/file

# Stage all changes
git add .

# Commit with message
git commit -m "Describe your changes"
```

### Push to Remote

```bash
# Push current branch
git push origin feature/my-feature

# Push all branches
git push --all

# Force push (use with caution!)
git push --force-with-lease
```

### Pull Changes

```bash
# Pull with rebase (recommended)
git pull --rebase

# Pull normally
git pull
```

### View History

```bash
# View commit log
git log

# View log with graph
git log --oneline --graph --all

# View commits by author
git log --author="Name"
```

## Git Best Practices

### Commit Messages

Write clear, descriptive commit messages:

```bash
# Good
git commit -m "Fix authentication bug in login flow"

# Bad
git commit -m "Fix bug"
```

Follow the format:
- **First line**: Short summary (50 chars max)
- **Blank line**
- **Body**: Explain what and why (wrap at 72 chars)

### Branch Naming

Use descriptive branch names:

```bash
# Features
git checkout -b feature/user-authentication

# Bug fixes
git checkout -b bugfix/login-redirect

# Improvements
git checkout -b improvement/performance-optimization

# Release
git checkout -b release/v1.2.0
```

### Keeping History Clean

```bash
# Rebase before pushing to clean up commits
git rebase -i origin/main

# Squash commits
git rebase -i HEAD~3  # Interactive rebase last 3 commits

# Amend last commit
git commit --amend --no-edit
```

## Git Environment Variables

Set these in your shell configuration:

```bash
# Git editor
export GIT_EDITOR=vim

# Avoid being asked for credentials repeatedly
export GIT_ASKPASS=/usr/bin/git-credential-osxkeychain  # macOS
```

## Helpful Git Commands

```bash
# Check status
git status

# Show unstaged changes
git diff

# Show staged changes
git diff --cached

# Show changes in specific file
git diff path/to/file

# Stash changes temporarily
git stash

# Apply stashed changes
git stash pop

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Discard last commit completely
git reset --hard HEAD~1

# View who changed each line
git blame path/to/file

# Search commit history
git log -S "search term"
```

## SSH Integration with Git

Ensure your SSH key is configured for Git authentication:

```bash
# Test SSH connection to GitHub
ssh -T git@github.com

# Should output: Hi USERNAME! You've successfully authenticated...
```

Your SSH configuration from the [SSH section](../ssh/index.md) enables secure Git operations without storing credentials.
