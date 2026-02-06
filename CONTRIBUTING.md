# Contributing to agentlib.sh

Thank you for your interest in contributing to agentlib.sh! This guide will help you submit high-quality resources.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Resource Structure](#resource-structure)
- [Writing Guidelines](#writing-guidelines)
- [Submission Process](#submission-process)
- [Review Criteria](#review-criteria)

## 🚀 Getting Started

### Prerequisites

- GitHub account
- Basic understanding of Markdown and YAML
- Familiarity with Cursor or Claude Code

### Fork and Clone

1. Fork this repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/agentlib.sh.git
   cd agentlib.sh
   ```

## 📁 Resource Structure

Each resource consists of two files in a dedicated folder:

```
resources/{type}/{platform}/{resource-name}/
├── content.md       # The actual rule/command content
└── metadata.yaml    # Metadata (name, description, tags, etc.)
```

### Path Components

- **type**: `rules` or `commands`
- **platform**: `cursor` or `claude-code`
- **resource-name**: URL-friendly name (lowercase, hyphens)

### Example

```
resources/rules/cursor/conventional-commits/
├── content.md
└── metadata.yaml
```

## 📝 Writing Guidelines

### metadata.yaml

Required fields:

```yaml
slug: unique-identifier            # Required: URL-friendly, unique across all resources
name: Display Name                 # Required: Human-readable name
description: |                     # Required: Clear explanation (2-4 sentences)
  What this resource does and why it's useful.

# Optional but recommended
tagline: Short one-liner           # Optional: Displayed in search results
problem_it_solves: |               # Optional: What problem does this solve?
  Explanation of the pain point this addresses.
source_url: https://example.com    # Optional: Reference or documentation URL
command_syntax: /command-name      # Required for commands only

# Tags (required)
tags:
  technology: [git, typescript]    # Technologies/tools this relates to
  lifecycle: [development]         # Development phases (development, testing, deployment, etc.)
  subject: [commits, testing]      # Topics/subjects covered
```

### content.md

Guidelines for writing clean content:

1. **No frontmatter**: Keep metadata in `metadata.yaml`
2. **Start with a clear title**: Use `# Title`
3. **Include examples**: Show concrete usage
4. **Be concise**: Get to the point quickly
5. **Use proper formatting**: Code blocks, lists, emphasis

#### For Rules

```markdown
# Rule Title

Brief explanation of what this rule enforces.

## Why use this?

Explain the benefits and use cases.

## Examples

### Good ✅
[Example of good code]

### Bad ❌
[Example of what to avoid]

## When to use

- Situation 1
- Situation 2
```

#### For Commands

```markdown
# Command Title

Brief description of what the command does.

## Usage

```
/command-name <arguments>
```

## What it does

1. Step 1
2. Step 2

## Examples

[Concrete examples]

## When to use / When NOT to use

[Guidance on appropriate usage]
```

### Naming Conventions

- **Folder names**: `lowercase-with-hyphens`
- **Slugs**: Same as folder name (e.g., `conventional-commits`)
- **File names**: Always `content.md` and `metadata.yaml`

### Tags Guidelines

Choose appropriate tags from these categories:

**Technology tags** (tools, languages, frameworks):
- `git`, `typescript`, `javascript`, `python`, `react`, `vue`, `node`, `docker`, etc.

**Lifecycle tags** (when to use):
- `development`, `testing`, `deployment`, `debugging`, `refactoring`, `documentation`

**Subject tags** (what it's about):
- `commits`, `code-quality`, `security`, `performance`, `testing`, `api`, `database`, etc.

## 🔄 Submission Process

### 1. Create a Branch

```bash
git checkout -b add-resource/your-resource-name
```

### 2. Create Your Resource

```bash
# Create the folder structure
mkdir -p resources/rules/cursor/your-resource-name

# Create the files
touch resources/rules/cursor/your-resource-name/content.md
touch resources/rules/cursor/your-resource-name/metadata.yaml
```

### 3. Write Content

- Fill in `metadata.yaml` with all required fields
- Write clear, helpful content in `content.md`
- Test your resource in Cursor/Claude Code

### 4. Commit and Push

```bash
git add resources/
git commit -m "feat: add your-resource-name rule for Cursor"
git push origin add-resource/your-resource-name
```

### 5. Create Pull Request

- Go to GitHub and create a PR from your branch
- Use a clear title: `Add: [Resource Name] for [Platform]`
- Describe what the resource does and why it's useful
- Wait for review feedback

## ✅ Review Criteria

Your submission will be evaluated on:

### Quality

- [ ] Clear, understandable content
- [ ] Practical and useful
- [ ] Well-formatted markdown
- [ ] Includes examples
- [ ] Free of typos and errors

### Completeness

- [ ] Both `content.md` and `metadata.yaml` present
- [ ] All required metadata fields filled
- [ ] Appropriate tags selected
- [ ] Unique slug

### Originality

- [ ] Not a duplicate of existing resources
- [ ] Adds value to the registry
- [ ] Solves a real problem

### Testing

- [ ] Resource works as described
- [ ] Tested in target platform (Cursor/Claude Code)
- [ ] Examples are accurate

## 🎨 Style Guide

### Markdown

- Use ATX-style headers (`#`, `##`, `###`)
- Use fenced code blocks with language identifiers
- Use bullet lists for unordered items
- Use numbered lists for sequential steps

### Writing Tone

- **Clear and direct**: Get to the point
- **Helpful and friendly**: Assume good intent
- **Professional**: Avoid slang and informal language
- **Inclusive**: Use gender-neutral language

### Code Examples

- Use realistic examples
- Show both good and bad patterns (when relevant)
- Include comments for clarity
- Keep examples concise

## ❓ Questions?

- **Issue tracker**: [GitHub Issues](https://github.com/YOUR_ORG/agentlib.sh/issues)
- **Discussions**: [GitHub Discussions](https://github.com/YOUR_ORG/agentlib.sh/discussions)

## 🙏 Thank You!

Every contribution makes agentlib.sh better for the entire AI coding community. We appreciate your time and effort!
