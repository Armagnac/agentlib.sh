# Conventional Commits Rule

When writing commit messages, follow the Conventional Commits specification:

## Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Types

- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that don't affect code meaning (white-space, formatting)
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `perf`: Code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `build`: Changes that affect the build system or external dependencies
- `ci`: Changes to CI configuration files and scripts
- `chore`: Other changes that don't modify src or test files
- `revert`: Reverts a previous commit

## Examples

Good commit messages:
```
feat(auth): add JWT token refresh mechanism
fix: resolve memory leak in event handler
docs: update API documentation for v2.0
refactor(api): simplify error handling logic
```

Bad commit messages (avoid these):
```
fixed stuff
WIP
update
asdf
small changes
```

## Breaking Changes

For breaking changes, add `!` after the type/scope:
```
feat(api)!: change response format to JSON:API spec

BREAKING CHANGE: API responses now follow JSON:API specification
```
