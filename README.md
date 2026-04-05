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
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT">
  <img src="https://img.shields.io/badge/tea_CLI-v0.10%2B-orange.svg" alt="tea CLI: v0.10+">
  <img src="https://img.shields.io/badge/agents-45%2B_supported-purple.svg" alt="Agents: 45+ supported">
</p>

---

## What it does

Teaches AI coding agents to manage [Gitea](https://gitea.io) via the [`tea` CLI](https://gitea.com/gitea/tea) — no hallucinated flags, just exact tested workflows.

| Capability | Operations |
|:-----------|:-----------|
| **Repositories** | Create, fork, clone, search, migrate, delete |
| **Issues** | Create, list, edit, close, comment, filter |
| **Pull Requests** | Create, review, approve, merge, checkout |
| **Releases** | Create with assets, edit, manage artifacts |
| **CI/CD Actions** | View runs, stream logs, manage secrets |
| **Labels & Milestones** | Full CRUD, sprint planning |
| **Organizations** | Create, list, manage |
| **Webhooks** | Create, update, delete |
| **Time Tracking** | Log time, view tracked hours |
| **Notifications** | List, read, pin, filter |
| **Direct API** | Raw `tea api` access to any endpoint |

## Installation

```bash
# Claude Code (recommended)
skills add gitea-skill

# Universal
npx @anthropic-ai/skills add gitea-skill

# Manual
git clone https://gitea.com/pk/gitea-skill.git
# Copy skills/gitea/ into your agent's skills directory
```

## Prerequisites

1. **Install tea CLI**

   ```bash
   brew install tea                           # macOS
   go install code.gitea.io/tea@latest        # From source
   ```

2. **Add a Gitea login**

   ```bash
   tea logins add --name my-gitea --url https://gitea.example.com --token YOUR_TOKEN
   ```

3. **Verify**: `tea whoami`

## Usage

Ask your agent any of these:

```
"Create a new repo called api-gateway in my org"
"List all open bugs assigned to me"
"Create a PR from this branch to main"
"Squash merge PR #42"
"Tag a v2.0.0 release with the changelog"
"Show me the CI logs for the latest run"
"Track 2 hours on issue #15"
```

## Supported Agents

Works with any AI coding agent with Bash access — Claude Code, Cursor, Cline, Codex, Windsurf, Roo Code, GitHub Copilot, Gemini CLI, and 40+ more via [skills CLI](https://github.com/vercel-labs/skills).

## Contributing

1. Fork → edit `skills/gitea/` → test with your agent → submit a PR

See [SKILL.md](skills/gitea/SKILL.md) for the full skill specification and [references/](skills/gitea/references/) for detailed docs.

## License

[MIT](LICENSE)
