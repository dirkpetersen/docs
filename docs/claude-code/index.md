# Claude Code

Documentation for Claude Code, an AI-powered development tool integrated into this workflow.

## Overview

Claude Code provides AI-assisted development capabilities for code generation, analysis, and problem-solving. This setup uses AWS Bedrock for model inference, enabling local-first Claude Code execution.

## Setup

### 1. Configure AWS Credentials

Add your AWS credentials to `~/.aws/credentials`:

```ini
[bedrock]
aws_access_key_id = XXXXXXXXXXX
aws_secret_access_key = YYYYYYYYYYYYYYYYY
```

### 2. Configure AWS Region

Add the Bedrock profile to `~/.aws/config`:

```ini
[profile bedrock]
region = us-west-2
```

### 3. Install Claude Code

Install Claude Code globally via npm:

```bash
npm i -g @anthropic-ai/claude-code
```

### 4. Create Wrapper Script

Create a wrapper script at `~/bin/claude` to simplify usage and model selection:

```bash
#! /bin/bash

export CLAUDE_CODE_USE_BEDROCK=1

export AWS_REGION=us-west-2
export AWS_PROFILE=bedrock
export ANTHROPIC_MODEL="anthropic.claude-sonnet-4-5-20250929-v1:0"
export ANTHROPIC_SMALL_FAST_MODEL="us.anthropic.claude-haiku-4-5-20251001-v1:0"

mymodel=${ANTHROPIC_SMALL_FAST_MODEL}
if [[ "$1" == "opus" ]]; then
  mymodel=$1
elif [[ "$1" == "sonnet" ]]; then
  mymodel="anthropic.claude-sonnet-4-5-20250929-v1:0"
fi

~/.claude/local/claude --dangerously-skip-permissions --model $mymodel $@
```

### 5. Make Script Executable and Add to PATH

First, create the `~/bin` directory if it doesn't exist:

```bash
mkdir -p ~/bin
```

Make the script executable:

```bash
chmod +x ~/bin/claude
```

Add `~/bin` to your PATH by adding this to your shell configuration (`.bashrc`, `.zshrc`):

```bash
export PATH="$HOME/bin:$PATH"
```

Reload your shell:

```bash
source ~/.bashrc  # or ~/.zshrc
```

### 6. Test Claude Code

Test that everything is set up correctly:

```bash
# Create a test directory
mkdir -p ~/test-project
cd ~/test-project
git init

# Launch Claude Code with default Haiku model
claude .
```

When Claude Code launches for the first time, it will:
1. Ask for your SSH keychain passphrase (cached for 4 hours)
2. Initialize the Claude Code environment
3. Start the interactive session in the git repository

## Important: Git Repository Requirement

**Claude Code must always be initialized inside a git repository.** This is a requirement for the tool to function properly.

### Setting Up a Git Repository

You have two options:

#### Option 1: Create a Local Repository

```bash
mkdir my-project
cd my-project
git init
```

Then run Claude Code inside this directory:

```bash
claude .
```

#### Option 2: Create a Repository on GitHub

1. Visit [github.com/new](https://github.com/new) to create a new repository
2. Clone it to your local machine:

```bash
git clone https://github.com/YOUR_USERNAME/my-project.git
cd my-project
```

3. Run Claude Code:

```bash
claude .
```

## Model Comparison

Understanding the differences between the three available models helps you choose the right tool for each task:

| Aspect | Haiku | Sonnet | Opus |
|--------|-------|--------|------|
| **Speed** | ⚡ Fast | ⚡⚡ Medium | 🐌 Slow |
| **Cost** | 💰 $0.80/MTok | 💰💰 $3.00/MTok | 💰💰💰 $15.00/MTok |
| **Performance** | Excellent | Very Good | Superior |
| **Complex Tasks** | ⭐ | ⭐⭐⭐ Medium | ⭐⭐⭐⭐⭐ High |
| **Coding Skills** | ⭐⭐ Medium | ⭐⭐⭐ Good | ⭐⭐⭐⭐⭐ Excellent |

## Usage

### Default (Haiku - Fast)

Run Claude Code with the default fast Haiku model for quick fixes and simple tasks:

```bash
claude /path/to/project
```

**Best for:**
- Quick bug fixes
- Simple code generation
- Refactoring small sections
- Cost-conscious tasks

### Sonnet (Balanced)

Run with the balanced Sonnet model for most development work:

```bash
claude sonnet /path/to/project
```

**Best for:**
- Complex feature development
- Detailed code analysis
- Medium-complexity problems
- Good balance of speed and capability

### Opus (Most Capable)

Run with the most capable Opus model for challenging problems:

```bash
claude opus /path/to/project
```

**Best for:**
- Difficult architectural decisions
- Complex problem-solving
- Full-project analysis
- Maximum capability required

## Configuration Notes

- **Bedrock Integration**: Uses AWS Bedrock for model inference
- **Context Window**: Defaults to 1M context window for Sonnet
- **No Confirmation**: Configured to skip permission prompts for streamlined workflow
- **Model Selection**: Easy switching between Haiku (fast), Sonnet (balanced), and Opus (capable)

## Tips and Best Practices

- Use **Haiku** for quick fixes and simple tasks
- Use **Sonnet** for complex development and analysis
- Use **Opus** for challenging problems requiring maximum capability
- Ensure AWS credentials are properly configured before use
- The wrapper script automatically handles model selection and AWS environment setup
