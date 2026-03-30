<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/go-gitea/gitea/main/assets/logo.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/go-gitea/gitea/main/assets/logo.svg">
    <img alt="Gitea" src="https://raw.githubusercontent.com/go-gitea/gitea/main/assets/logo.svg" width="120">
  </picture>
  <br>
  <strong>gitea-skill</strong>
  <br>
  <em>An agent skill for managing Gitea instances via the <a href="https://gitea.com/gitea/tea">tea CLI</a></em>
</p>

<p align="center">
  <a href="#installation">Installation</a> &bull;
  <a href="#what-it-does">What it does</a> &bull;
  <a href="#supported-agents">Supported agents</a> &bull;
  <a href="#prerequisites">Prerequisites</a> &bull;
  <a href="#examples">Examples</a> &bull;
  <a href="#contributing">Contributing</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT">
  <img src="https://img.shields.io/badge/skill-v1.0.0-green.svg" alt="Version: 1.0.0">
  <img src="https://img.shields.io/badge/tea_CLI-v0.10%2B-orange.svg" alt="tea CLI: v0.10+">
  <img src="https://img.shields.io/badge/agents-45%2B_supported-purple.svg" alt="Agents: 45+ supported">
</p>

---

## What it does

**gitea-skill** teaches AI coding agents how to interface with [Gitea](https://gitea.io) instances through the [`tea` CLI](https://gitea.com/gitea/tea). Instead of the agent guessing at commands or hallucinating flags, this skill provides exact, tested workflows for every Gitea operation.

| Capability | Operations |
|:-----------|:-----------|
| **Repositories** | Create, fork, clone, search, migrate, delete |
| **Issues** | Create, list, edit, close, reopen, comment, filter |
| **Pull Requests** | Create, review, approve, reject, merge, checkout, clean |
| **Releases** | Create with assets, edit, manage release artifacts |
| **CI/CD Actions** | View runs, stream logs, manage secrets & variables |
| **Labels & Milestones** | Full CRUD, sprint planning workflows |
| **Organizations** | Create, list, manage |
| **Webhooks** | Create, update, delete (Gitea, Slack, Discord, etc.) |
| **Time Tracking** | Log time, view tracked hours |
| **Notifications** | List, read, pin, filter |
| **Direct API** | Raw `tea api` access to any Gitea endpoint |

## Installation

### Claude Code

```bash
# via skills CLI (recommended)
skills add gitea-skill

# or manually: clone into your skills directory
git clone https://gitea.com/pk/gitea-skill.git ~/.claude/skills/gitea
```

### Cursor / Cline / Other Agents

```bash
# install via the universal skills CLI
npx @anthropic-ai/skills add gitea-skill
```

### Manual Installation

```bash
git clone https://gitea.com/pk/gitea-skill.git
# Copy skills/gitea/ into your agent's skills directory
```

## Prerequisites

1. **Install tea CLI**

   ```bash
   # macOS
   brew install tea

   # Linux (prebuilt binary)
   curl -sL https://dl.gitea.io/tea/latest/tea-latest-linux-amd64 -o /usr/local/bin/tea && chmod +x /usr/local/bin/tea

   # From source
   go install code.gitea.io/tea@latest
   ```

2. **Add a Gitea login**

   ```bash
   # Token-based (recommended)
   tea logins add --name my-gitea --url https://gitea.example.com --token YOUR_TOKEN

   # OAuth2 (browser flow)
   tea logins add --name my-gitea --url https://gitea.example.com --oauth
   ```

3. **Verify**

   ```bash
   tea whoami
   ```

## Skill Structure

```
skills/gitea/
├── SKILL.md                          # Core instructions (auto-loaded)
├── references/
│   ├── tea-commands.md               # Complete CLI flag reference
│   ├── authentication.md             # Auth setup & multi-instance guide
│   └── workflows.md                  # Advanced workflows & API patterns
└── scripts/
    └── check-tea.sh                  # Setup validation script
```

The skill uses **progressive disclosure** to minimize token usage:

| Level | File | Loaded when |
|:------|:-----|:------------|
| 1 | Frontmatter | Always (484 chars in system prompt) |
| 2 | SKILL.md body | When skill matches user request |
| 3 | `references/*` | On demand, when agent needs detail |

## Examples

Ask your agent any of these:

```
"Create a new repo called api-gateway in my org"
"List all open bugs assigned to me"
"Create a PR from this branch to main"
"Squash merge PR #42"
"Tag a v2.0.0 release with the changelog"
"Set up a webhook for push events"
"Show me the CI logs for the latest run"
"Fork upstream/project and create a PR for my fix"
"Track 2 hours on issue #15"
"What notifications do I have?"
```

## Supported Agents

This skill works with any AI coding agent that has Bash/shell access, including:

| Agent | Status |
|:------|:-------|
| **Claude Code** | Fully supported |
| **Cursor** | Fully supported |
| **Cline** | Fully supported |
| **Codex** | Fully supported |
| **Windsurf** | Fully supported |
| **Roo Code** | Fully supported |
| **GitHub Copilot** | Fully supported |
| **Gemini CLI** | Fully supported |
| 40+ others | Fully supported via [skills CLI](https://github.com/vercel-labs/skills) |

## How It Works

```
User: "Create a bug report for the login timeout and assign it to alice"
         │
         ▼
   ┌─────────────┐
   │  Agent sees  │    Frontmatter triggers on "Gitea", "issue",
   │  SKILL.md    │◄── "create", "assign" keywords
   │  frontmatter │
   └──────┬──────┘
          │
          ▼
   ┌─────────────┐
   │  Agent loads │    Full instructions with exact tea commands,
   │  SKILL.md    │◄── flags, and best practices
   │  body        │
   └──────┬──────┘
          │
          ▼
   ┌─────────────┐
   │  Agent runs  │    tea issues create --title "Bug: login timeout"
   │  tea command │◄──   --labels bug --assignees alice
   └─────────────┘
```

## Gitea + tea Compatibility

| Gitea Version | tea Version | Status |
|:--------------|:------------|:-------|
| 1.18+ | 0.10+ | Full support |
| 1.20+ | 0.10+ | Full support + Actions |
| 1.22+ | 0.12+ | Full support + Actions enhancements |

## Configuration

tea stores its configuration at `~/.config/tea/config.yml`. The skill auto-detects logins and repo context from git remotes. For multi-instance setups:

```yaml
# ~/.config/tea/config.yml
logins:
  - name: work
    url: https://gitea.company.com
    token: "..."
    default: true

  - name: personal
    url: https://gitea.example.com
    token: "..."

preferences:
  flag_defaults:
    remote: upstream    # prefer upstream remote for context detection
```

## Contributing

Contributions welcome! To improve this skill:

1. Fork the repository
2. Edit files in `skills/gitea/`
3. Test with your agent of choice
4. Submit a PR

### Testing Checklist

- [ ] Skill triggers on relevant queries ("create gitea issue", "merge PR", etc.)
- [ ] Skill does NOT trigger on unrelated queries ("write Python code", "help with GitHub")
- [ ] Commands produce correct output
- [ ] Error handling covers common failure modes
- [ ] Works without repo context (using `--repo` and `--login` flags)

## Related

- [tea CLI](https://gitea.com/gitea/tea) - The official Gitea CLI
- [Gitea](https://gitea.io) - Self-hosted Git service
- [Gitea API docs](https://gitea.com/api/swagger) - Full REST API reference
- [Agent Skills](https://github.com/vercel-labs/skills) - Universal skill installer for AI agents
- [Building Skills for Claude](https://docs.anthropic.com/en/docs/agents-and-tools/skills) - Anthropic's skill documentation

## License

[MIT](LICENSE)
