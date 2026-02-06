# agentlib.sh

> A curated registry of rules and commands for AI coding agents (Cursor, Claude Code)

This repository contains the source content for [agentlib.sh](https://agentlib.sh) - an open registry of reusable rules and commands for AI coding agents.

## 🎯 What is this?

agentlib.sh is a community-driven collection of:

- **Rules**: Guidelines and constraints that shape how AI agents write code
- **Commands**: Reusable shortcuts and automations for common tasks

These resources help you get more consistent, high-quality output from AI coding assistants.

## 📁 Repository Structure

```
agentlib.sh/
├── resources/
│   ├── rules/
│   │   ├── cursor/           # Rules for Cursor editor
│   │   └── claude-code/      # Rules for Claude Code
│   └── commands/
│       ├── cursor/           # Commands for Cursor editor
│       └── claude-code/      # Commands for Claude Code
```

Each resource is a folder containing:
- `content.md` - The actual rule/command content (clean, no metadata)
- `metadata.yaml` - Name, description, tags, and other metadata

## 🚀 Using Resources

### Browse on the Web

Visit [agentlib.sh](https://agentlib.sh) to browse, search, and discover resources.

### Install Directly

Each resource can be installed via curl:

```bash
# Example: Install the Conventional Commits rule
curl https://raw.githubusercontent.com/YOUR_ORG/agentlib.sh/main/resources/rules/cursor/conventional-commits/content.md > .cursor/rules/conventional-commits.md
```

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

### Quick Start

1. Fork this repository
2. Create a new resource folder:
   ```
   resources/rules/cursor/your-resource-name/
   ├── content.md
   └── metadata.yaml
   ```
3. Write your content and metadata
4. Submit a pull request

### Resource Quality Guidelines

- **Clear and focused**: Each resource should solve one specific problem
- **Well-documented**: Include examples and use cases
- **Tested**: Verify your resource works as expected
- **Properly tagged**: Use appropriate technology, lifecycle, and subject tags

## 📝 Resource Format

### metadata.yaml

```yaml
slug: unique-identifier
name: Display Name
tagline: Short one-liner description
description: |
  Longer description explaining what this resource does
  and why it's useful.
problem_it_solves: |
  Explanation of the problem this solves.
source_url: https://optional-reference.com
command_syntax: /optional-command-syntax  # For commands only
tags:
  technology: [git, typescript, react]
  lifecycle: [development, testing, deployment]
  subject: [commits, testing, documentation]
```

### content.md

Clean markdown with/out frontmatter:

```markdown
# Your Resource Title

Clear explanation of what it does...

## Examples

Concrete examples...
```

## 📊 Statistics

- 2+ resources available
- Supports Cursor and Claude Code
- Community-driven and open source

## 🔗 Links

- Website: [agentlib.sh](https://agentlib.sh)

---

Made with ❤️ by the Armagnac & AI coding community
