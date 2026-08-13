# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.1.0] - 2026-08-13

Retargeted to tea CLI v0.15+ (tested on 0.15.1). Every documented command was checked against that CLI.

### Added

- Wiki, SSH keys, comments CRUD, PR review threads, workflow dispatch, admin users, git credential helper

### Changed

- Command forms updated for tea 0.15.1 (`tea org`, release tags, `--id` labels, milestone titles, `repos delete --owner/--name`, `repos fork --repo`, …)
- Dropped stale v0.10 forms that now fail (`tea orgs`, `tea pulls ls --labels`, numeric release IDs, `webhooks create --url`, `--service github`)

## [1.0.0] - 2026-03-30

### Added

- Initial release
- Core SKILL.md with workflows for issues, PRs, repos, releases, actions, labels, milestones, orgs, webhooks, time tracking, notifications
- Complete tea CLI command reference (`references/tea-commands.md`)
- Authentication guide with token, OAuth, SSH, and multi-instance setup (`references/authentication.md`)
- Advanced workflow patterns including fork-based PRs, sprint planning, bulk operations, and 20+ `tea api` endpoint examples (`references/workflows.md`)
- Setup validation script (`scripts/check-tea.sh`)
- Progressive disclosure: 484-char frontmatter, 1,600-word SKILL.md body, detailed references on demand
- Negative triggers to avoid false activation on GitHub/GitLab tasks
- 5 troubleshooting entries covering common failure modes
- 4 worked examples with realistic user prompts
- Support for 45+ AI coding agents via universal skills format
