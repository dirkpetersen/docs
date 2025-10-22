# Dirk Petersen's Development Configuration

Welcome to a comprehensive guide for setting up and working with a modern, security-focused development environment. This documentation covers everything from foundational shell configuration to cloud platform integration.

## Quick Start

New to this setup? Start here:

1. **[Shell](shell/index.md)** - Configure your Unix environment with SSH multiplexing and keychain
2. **[GitHub](github/index.md)** - Set up Git authentication using GitHub CLI
3. **[Claude Code](claude-code/index.md)** - Configure AI-assisted development with AWS Bedrock
4. **[Python](python/index.md)** - Set up Python development with UV or Pixi

## Complete Documentation

### Foundation & Tools

- **[Shell](shell/index.md)** - SSH key management, Git configuration, WSL setup, shell profiles
- **[GitHub](github/index.md)** - GitHub CLI, SSH authentication, basic Git workflows

### Development

- **[Claude Code](claude-code/index.md)** - AI-powered development with Bedrock, model comparison (Haiku/Sonnet/Opus), git repository requirements
- **[Python](python/index.md)** - Indigo Python distribution, UV package manager, Pixi for conda compatibility

### Infrastructure

- **[Nvidia](nvidia/index.md)** - GPU acceleration, CUDA, and compute optimization
- **[Clouds](clouds/index.md)** - Multi-cloud setup for AWS, Azure, and Google Cloud

## Key Features

### Security-First Design

- 🔐 SSH keys with passphrase protection and keychain caching
- 🎯 Service-specific AWS profiles with least-privilege IAM policies
- ✅ GitHub CLI for streamlined authentication
- 🔄 SSH multiplexing for efficient, reliable connections

### Developer Experience

- ⚡ Fast model switching for Claude Code (Haiku → Sonnet → Opus)
- 📦 Modern Python packaging with UV (fast) or Pixi (conda-compatible)
- 🌐 Multi-cloud support with consistent credential patterns
- 🛠️ Comprehensive shell aliases and environment optimization

### Best Practices

- Proper credential scope isolation
- Temporary credentials where possible
- Comprehensive audit logging
- Regular credential rotation
- Clear permission boundaries

## Documentation Structure

Each section includes:

- **Setup Instructions** - Step-by-step configuration
- **Configuration Files** - Annotated examples and explanations
- **Security Guidelines** - Best practices for the platform
- **Common Use Cases** - Real-world examples
- **Troubleshooting** - Solutions to common issues

## About This Site

This documentation is designed for teams and individuals who want to:

- Understand and replicate this development environment
- Learn security best practices for cloud and local development
- Build reliable, consistent development workflows
- Work productively with AI-assisted tools like Claude Code
- Maintain clear separation of concerns across multiple services

Whether you're setting up for the first time or optimizing an existing workflow, this guide provides practical, tested configurations and explanations.

## Getting Help

For issues or questions:

- Check the specific section's troubleshooting or best practices
- Review configuration examples provided in each guide
- Ensure you're following the security-first approach outlined
- Verify file permissions match the documented settings
