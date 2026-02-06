# Quick Commit Command

Stage all changes and commit in one command.

## Usage

```
/commit <message>
```

## What it does

1. Stages all modified and new files (`git add .`)
2. Creates a commit with your message (`git commit -m "<message>"`)

## Examples

```bash
# Quick commit during development
/commit "add user authentication"

# Fix and commit
/commit "fix: resolve null pointer in login handler"

# Feature commit
/commit "feat: implement password reset flow"
```

## When to use

- During rapid prototyping
- Making many small commits
- Quick fixes that don't need selective staging

## When NOT to use

- When you need to stage files selectively
- For complex commits with multiple logical changes
- When you need to review staged changes before committing

## Pro tip

Combine with conventional commit format for best results:
```
/commit "feat(auth): add social login support"
/commit "fix(ui): correct button alignment on mobile"
```
