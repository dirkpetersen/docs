# GitHub

Essential setup and configuration for working with GitHub repositories.

## SSH Key Setup

Using SSH keys provides secure, password-free authentication with GitHub. This guide covers generating and configuring SSH keys.

### Generate SSH Key

Generate a new SSH key pair:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519
```

When prompted for a passphrase, **enter a strong password** to protect your key. You'll enter this password once and it will be stored in your system keychain.

### Add SSH Key to GitHub

1. Display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

2. Copy the entire output

3. Go to GitHub Settings → SSH and GPG keys → [New SSH key](https://github.com/settings/ssh/new)

4. Paste your public key and give it a descriptive title (e.g., "My Laptop")

5. Click "Add SSH key"

### Test SSH Connection

Verify your SSH setup:

```bash
ssh -T git@github.com
```

You should see:

```
Hi YOUR_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

## Using SSH Keychain to Store Passwords

Protect your SSH key passphrase by storing it in your system keychain. This way you only enter your password once per session.

### Linux with WSL

For WSL2 on Windows, use ssh-keychain to manage your SSH key passphrase:

1. **Install ssh-keychain**

```bash
sudo apt-get update
sudo apt-get install ssh-askpass ssh-keychain
```

2. **Add to your shell configuration** (`.bashrc`, `.zshrc`, etc.)

```bash
# SSH Keychain - unlocks SSH key passphrase once per session
eval $(ssh-keychain -q -t 4h ~/.ssh/id_ed25519)
```

3. **Reload your shell**

```bash
source ~/.bashrc  # or ~/.zshrc
```

4. **First SSH command of the session**

On your first SSH/Git command of the session, you'll be prompted for your SSH key passphrase. Enter it once and it will be cached in keychain for the next 4 hours.

### Reference

For more details on SSH keychain setup with WSL:
https://askubuntu.com/questions/1356352/install-ssh-keychain-on-ubuntu-with-wsl

## Basic Git Workflow

### Clone a Repository

```bash
git clone git@github.com:USERNAME/repository.git
cd repository
```

### Create and Switch Branches

```bash
git checkout -b feature/my-feature
```

### Make Changes and Commit

```bash
git add .
git commit -m "Describe your changes"
```

### Push to GitHub

```bash
git push origin feature/my-feature
```

### Create a Pull Request

After pushing, visit your repository on GitHub and create a pull request to merge your branch.

## Cloning Your First Repository

Your first GitHub repository can be cloned locally:

```bash
git clone git@github.com:YOUR_USERNAME/your-repo.git
cd your-repo
```

Then you can run Claude Code inside:

```bash
claude .
```
