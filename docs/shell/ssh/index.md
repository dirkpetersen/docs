# Shell - SSH Configuration

SSH key management, keychain setup, and connection optimization.

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
sudo apt-get install ssh-askpass keychain
```

**On macOS:**

```bash
# Usually included by default
# If not: brew install keychain
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

## SSH Security Best Practices

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

### SSH Connection Best Practices

- Use SSH keys instead of passwords
- Keep SSH config clean and organized
- Test connections with `ssh -v` for debugging
- Monitor SSH access logs regularly
- Disable root login on remote servers
