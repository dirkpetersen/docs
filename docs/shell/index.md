# Shell

Unix shell configuration and environment setup for a productive development environment.

## Overview

This section covers essential shell setup including SSH configuration, terminal tools, and environment variables.

## SSH Key Management

Secure shell access is critical for development work. This section covers SSH setup with password-protected keys and keychain integration.

### Generating SSH Keys

SSH keys should always use a passphrase for security. When generating keys, use a strong password:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519
```

**Important**: When prompted for a passphrase, enter a strong password. This protects your key if it's ever compromised.

### SSH Key Permissions

Ensure proper permissions on your SSH keys:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

### SSH Configuration

Create or edit `~/.ssh/config` to simplify SSH connections and optimize performance:

```bash
# SSH Connection Multiplexing and Global Defaults
Host *
    ControlPath ~/.ssh/controlmasters/%r@%h:%p
    ControlMaster auto
    ControlPersist 10m
    ServerAliveInterval 10
    ServerAliveCountMax 3
    User your-username

# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    AddKeysToAgent yes

# Jump Host Example
Host jumphost
    HostName jump.example.com
    ControlMaster auto
    DynamicForward 1080

# Remote Server Behind Jump Host
Host remote-server
    HostName server.example.com
    ProxyJump jumphost
```

**Key Configuration Options:**

- `ControlPath`: Directory for multiplexed connections (creates `.ssh/controlmasters/` dir)
- `ControlMaster auto`: Automatically reuse connections
- `ControlPersist 10m`: Keep connections alive for 10 minutes
- `ServerAliveInterval 10`: Send keepalive every 10 seconds
- `ServerAliveCountMax 3`: Disconnect after 3 missed keepalives
- `ProxyJump`: Chain connections through jump host
- `DynamicForward`: Enable SOCKS proxy through host

**Setup multiplexing directory:**

```bash
mkdir -p ~/.ssh/controlmasters
chmod 700 ~/.ssh/controlmasters
```

**Benefits:**

- **Faster connections**: Reuses SSH connections, eliminates repeated authentication
- **Reliable**: Keepalive prevents timeouts on idle connections
- **Secure**: Jump host proxy keeps direct connections private
- **Flexible**: Works with any remote host configuration

## SSH Keychain

Use SSH keychain to securely store your SSH key passphrase in memory, eliminating the need to re-enter it for every SSH command.

### Installation

**On Ubuntu/WSL:**

```bash
sudo apt-get update
sudo apt-get install ssh-askpass ssh-keychain
```

**On macOS:**

```bash
# Usually included by default
# If not: brew install ssh-askpass
```

### Configuration

Add to your shell configuration file (`.bashrc`, `.zshrc`, or `.profile`):

```bash
# SSH Keychain - loads SSH key passphrase into memory
eval $(ssh-keychain -q -t 4h ~/.ssh/id_ed25519)
```

The `-t 4h` parameter sets the cache timeout to 4 hours. Adjust as needed.

### How It Works

1. On your first SSH/Git command of the session, you'll be prompted for your SSH key passphrase
2. Enter your password - it's stored securely in your system keychain
3. For the next 4 hours, all SSH connections use the cached passphrase
4. After the timeout, you'll be prompted again

This approach provides security (your key is protected with a password) and convenience (you only type it once per session).

### Troubleshooting

If prompted repeatedly for your passphrase:

```bash
# Check if ssh-agent is running
ps aux | grep ssh-agent

# Manually start ssh-agent
eval $(ssh-agent -s)

# Add key manually
ssh-add ~/.ssh/id_ed25519
```

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

## Terminal Best Practices

### Keep SSH Keys Secure

- Never commit SSH keys to version control
- Never share your private key
- Always use strong passphrases
- Rotate keys periodically
- Use key files only for automated systems

### SSH Agent Security

- Let keychain manage your passphrase
- Don't store unencrypted passphrases
- Lock your computer when stepping away
- Expire cached passphrases after reasonable time

### Command History

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

## Useful Shell Aliases

Add to your shell configuration:

```bash
# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -lah'
alias l='ls -la'

# Git
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log'

# Development
alias cls='clear'
alias py='python'
alias serve='python -m http.server'
```

## Secure Development Environment

Your shell environment should be:

- **Secure**: SSH keys protected with passphrases, keychain for convenience
- **Clean**: Well-organized bin directories, clear PATH
- **Productive**: Useful aliases, proper history management
- **Isolated**: Virtual environments for projects, clear separation of concerns
