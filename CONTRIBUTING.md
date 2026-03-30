# Contributing to gitea-skill

Thanks for your interest in improving this skill! Here's how to contribute effectively.

## Quick Start

1. Fork and clone the repository
2. Make your changes in `skills/gitea/`
3. Test with your preferred agent (Claude Code, Cursor, etc.)
4. Submit a PR

## Skill File Guidelines

### SKILL.md

- Keep under 5,000 words (currently ~1,600)
- Put detailed reference material in `references/`, not inline
- Every command example must be copy-pasteable with realistic flags
- Include both the command and what it does
- No vague instructions ("validate things properly") -- be specific

### Frontmatter

- `name`: kebab-case, must match folder name
- `description`: must include WHAT it does AND WHEN to trigger
- No XML angle brackets (`<` or `>`) anywhere in frontmatter
- Description under 1,024 characters

### References

- `tea-commands.md` -- Complete flag/option reference
- `authentication.md` -- Login and auth setup
- `workflows.md` -- Multi-step workflows and `tea api` patterns

### Scripts

- Must be POSIX-compatible or use `#!/usr/bin/env bash`
- Must be executable (`chmod +x`)
- Should have clear exit codes

## Testing

Before submitting, verify:

1. **Triggering** -- Your changes don't break skill activation
2. **Accuracy** -- All `tea` commands and flags are correct for tea v0.10+
3. **Cross-agent** -- Nothing is agent-specific (no Claude-only features)

### Manual Test Prompts

Try these with your agent after making changes:

```
"List my open Gitea issues"
"Create a PR from this branch"
"Show CI run logs"
"Set up a new Gitea repo with labels"
```

## What Makes a Good Contribution

- Fixing incorrect flags or command syntax
- Adding workflows for operations not yet covered
- Improving error handling / troubleshooting entries
- Adding `tea api` patterns for uncovered API endpoints
- Keeping up with new tea CLI releases

## What to Avoid

- Adding agent-specific logic (keep it universal)
- Putting large content blocks directly in SKILL.md (use references/)
- Adding a README.md inside the `skills/gitea/` folder
- Using XML tags in any skill file
