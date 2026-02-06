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

Each resource is a markdown file with frontmatter in a dedicated folder:

```
resources/{platform}/{type}/{resource-name}/content.md
```

### Path Components

- **platform**: `cursor` or `claude-code`
- **type**: Platform-specific types:
  - **Cursor**: `rules`, `commands`, `skills`, `subagents`
  - **Claude Code**: `commands`, `skills`, `agents`
- **resource-name**: URL-friendly name (lowercase, hyphens)

### Examples

```
resources/cursor/commands/quick-commit/content.md
resources/claude-code/skills/pdf-processing/content.md
resources/claude-code/agents/code-reviewer/content.md
```

**Note:** Skills may include additional supporting files (scripts, templates) in the same folder.

## 📝 Writing Guidelines

### Frontmatter Schema

All resources use YAML frontmatter at the top of `content.md`. The registry auto-generates slug, name, tagline, and tags.

**Commands:**
```yaml
---
description: Clear 2-4 sentence description
resource_type: command
command_syntax: /command-name
---
```

**Skills:**
```yaml
---
description: Clear 2-4 sentence description
resource_type: skill
---
```

**Agents/Subagents:**
```yaml
---
name: agent-name
description: When to use this agent
resource_type: agent
tools: Read, Grep, Bash
model: sonnet
---
```

**Rules (Cursor only):**
```yaml
---
description: Clear 2-4 sentence description
resource_type: rule
source_url: https://reference.com  # Optional
---
```

### Content Guidelines

Write clean, concise markdown after frontmatter:

1. **Start with frontmatter** - required metadata in YAML
2. **Clear title** - use `# Title`
3. **Include examples** - show concrete usage
4. **Be concise** - get to the point quickly

### Resource Type Specifics

| Type | What to Include | Install Location |
|------|----------------|------------------|
| **Commands** | Brief prompt/workflow, usage examples, when to use | `.cursor/commands/` or `.claude/commands/` |
| **Skills** | Capability description, supporting files (scripts, templates), usage guide | `.cursor/skills/*/SKILL.md` or `.claude/skills/*/SKILL.md` |
| **Agents/Subagents** | System prompt, expertise domain, tool restrictions, when to invoke | `.cursor/agents/` or `.claude/agents/` |
| **Rules** (Cursor only) | Code guidelines, formatting rules, conventions to follow | `.cursor/rules/` |

**Commands:**
```markdown
---
description: Stage all changes and commit with one command
resource_type: command
command_syntax: /commit <message>
---

# Quick Commit
Brief description and usage...
```

**Skills:**
```markdown
---
description: Process PDF files and extract text
resource_type: skill
---

# PDF Processing
How to use this skill and supporting files...
```

**Agents/Subagents:**
```markdown
---
name: code-reviewer
description: Expert code review specialist
resource_type: agent
tools: Read, Grep, Bash
model: sonnet
---

You are an expert code reviewer.

Focus on:
- Security vulnerabilities
- Performance implications
```

### Naming Conventions

**Naming:**
- Folders: `lowercase-with-hyphens`
- File: Always `content.md`
- Slug: Auto-generated from folder name

**Auto-generated Fields:**

The registry sync system automatically generates:
- **slug**: From folder path
- **name**: From folder name (titlecased)
- **tagline**: Via LLM from description
- **tags**: Via LLM from content analysis (technology, lifecycle, subject)

## 🔄 Submission Process

### 1. Create a Branch

```bash
git checkout -b add-resource/your-resource-name
```

### 2. Create Your Resource

```bash
# Example: Create a command
mkdir -p resources/cursor/commands/your-command-name
touch resources/cursor/commands/your-command-name/content.md

# Example: Create a skill (with supporting files)
mkdir -p resources/claude-code/skills/your-skill-name
touch resources/claude-code/skills/your-skill-name/content.md
# Add supporting scripts/templates as needed
```

### 3. Write Content

- Add frontmatter with required fields to `content.md`
- Write clear, helpful markdown content
- Test your resource in Cursor/Claude Code

### 4. Commit and Push

```bash
git add resources/
git commit -m "feat: add your-resource-name {type} for {platform}"
git push origin add-resource/your-resource-name
```

### 5. Create Pull Request

- Go to GitHub and create a PR from your branch
- Use a clear title: `Add: [Resource Name] for [Platform]`
- Describe what the resource does and why it's useful
- Wait for review feedback

## ✅ Review Criteria

Your submission will be evaluated on:

- [ ] Clear, practical content with examples
- [ ] `content.md` with proper frontmatter
- [ ] All required frontmatter fields filled
- [ ] Resource works as described in target platform
- [ ] Not a duplicate, adds value
- [ ] Well-formatted markdown, no typos

## 🎨 Style Guide

**Markdown:**
- ATX-style headers (`#`, `##`)
- Fenced code blocks with language tags
- Bullet lists for unordered, numbered for sequential

**Tone:**
- Clear and direct
- Professional and inclusive

**Examples:**
- Realistic and concise
- Include comments for clarity

## ❓ Questions?

- **Issue tracker**: [GitHub Issues](https://github.com/YOUR_ORG/agentlib.sh/issues)
- **Discussions**: [GitHub Discussions](https://github.com/YOUR_ORG/agentlib.sh/discussions)

## 🙏 Thank You!

Every contribution makes agentlib.sh better for the entire AI coding community. We appreciate your time and effort!
