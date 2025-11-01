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

### SSH Keychain

Use SSH keychain to securely store your SSH key passphrase in memory, eliminating the need to re-enter it for every SSH command.

#### Installation

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

#### Configuration

Add to your login shell configuration file (`.profile` or `.bash_profile`, not `.bashrc` or `.zshrc`):

```bash
# SSH Keychain - loads SSH key passphrase into memory
# Only run on interactive login shells (not in Slurm jobs)
[ -z $SLURM_PTY_PORT ] && eval $(keychain --quiet --eval id_ed25519)
```

The keychain setup should be in `.profile` or `.bash_profile` because:
- These files run only on login shells (when you first log in)
- `.bashrc` and `.zshrc` run on every shell invocation (including non-interactive ones)
- This prevents redundant keychain initialization

**Important for HPC**: The `[ -z $SLURM_PTY_PORT ]` check prevents keychain from running inside Slurm job steps, where PTY management is handled by Slurm.

#### How It Works

1. On your first SSH/Git command after login, you'll be prompted for your SSH key passphrase
2. Enter your password - it's stored securely in your system keychain
3. The passphrase persists in the keychain **until you reboot your computer**
4. All subsequent SSH connections reuse the cached passphrase without prompting
5. After reboot, you'll be prompted for your passphrase again on first use

This approach provides both security (your key is protected with a passphrase) and exceptional convenience (you only type your passphrase once per boot, not per session).

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

## Troubleshooting

### SSH Key Permissions

Ensure proper permissions on your SSH keys - incorrect permissions can prevent SSH authentication:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

**Explanation:**
- `~/.ssh` directory: `700` (rwx------) - only owner can access
- Private key: `600` (rw-------) - only owner can read/write
- Public key: `644` (rw-r--r--) - readable by others, writable only by owner

### Keychain Not Working

If prompted repeatedly for your passphrase:

```bash
# Check if ssh-agent is running
ps aux | grep ssh-agent

# Manually start ssh-agent
eval $(ssh-agent -s)

# Add key manually
ssh-add ~/.ssh/id_ed25519
```

### SSH Connection Issues

- Test connections with `ssh -v` for verbose debugging
- Check SSH config syntax: `ssh -T git@github.com`
- Verify remote host is accepting your key
- Ensure firewall isn't blocking port 22

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

### SSH Connection Best Practices

- Use SSH keys instead of passwords
- Keep SSH config clean and organized
- Monitor SSH access logs regularly
- Disable root login on remote servers
