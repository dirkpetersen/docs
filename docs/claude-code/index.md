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

Create a wrapper script at `~/bin/claude` to simplify usage:

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

Make it executable:

```bash
chmod +x ~/bin/claude
```

## Usage

### Default (Haiku - Fast)

Run Claude Code with the default fast Haiku model:

```bash
claude /path/to/project
```

### Sonnet (Balanced)

Run with the more capable Sonnet model:

```bash
claude sonnet /path/to/project
```

### Opus (Most Capable)

Run with the most capable Opus model:

```bash
claude opus /path/to/project
```

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
