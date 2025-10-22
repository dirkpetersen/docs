# Shell - Basic Setup

Essential shell configuration for a productive Unix environment.

## Shell Profile Setup

### Bash Configuration

Edit `~/.bashrc`:

```bash
# Add SSH Keychain
eval $(ssh-keychain -q -t 4h ~/.ssh/id_ed25519)

# Add useful aliases
alias ll='ls -lah'
alias cd..='cd ..'
alias clear-cache='rm -rf ~/.cache'

# Set default editor
export EDITOR=vim

# Add local bin to PATH
export PATH="$HOME/.local/bin:$PATH"
```

### Zsh Configuration

Edit `~/.zshrc`:

```bash
# Add SSH Keychain
eval $(ssh-keychain -q -t 4h ~/.ssh/id_ed25519)

# Add useful aliases
alias ll='ls -lah'
alias cd..='cd ..'

# Set default editor
export EDITOR=vim

# Add local bin to PATH
export PATH="$HOME/.local/bin:$PATH"
```

## Essential Environment Variables

Set these in your shell configuration:

```bash
# Editor
export EDITOR=vim

# Language
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# Path additions
export PATH="$HOME/.local/bin:$HOME/.cargo/bin:$PATH"

# Git
export GIT_EDITOR=vim
```

## Useful Shell Aliases

Add to your shell configuration:

```bash
# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -lah'
alias l='ls -la'

# Development
alias cls='clear'
alias py='python'
alias serve='python -m http.server'
```

## Command History

Keep your shell history clean:

```bash
# Increase history size
export HISTSIZE=10000
export HISTFILESIZE=10000

# Ignore duplicate commands
export HISTCONTROL=ignoredups

# Ignore common commands from history
export HISTIGNORE="ls:cd:pwd:history"
```

## Secure Development Environment

Your shell environment should be:

- **Secure**: SSH keys protected with passphrases, keychain for convenience
- **Clean**: Well-organized bin directories, clear PATH
- **Productive**: Useful aliases, proper history management
- **Isolated**: Virtual environments for projects, clear separation of concerns
