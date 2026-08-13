# Tea CLI Complete Command Reference

Complete flag and option reference for tea v0.15+ (tested on 0.15.1). Consult this when you need exact flag names, aliases, or default values.

## Global Flags

These flags are available on ALL commands:

| Flag | Alias | Description |
|------|-------|-------------|
| `--debug` | `--vvv` | Enable debug output |
| `--help` | `-h` | Show help |
| `--version` | `-v` | Print version |

## Repository Context Flags

Available on most entity commands (issues, pulls, labels, etc.):

| Flag | Alias | Description |
|------|-------|-------------|
| `--repo` | `-r` | Override repository (owner/repo slug or local path) |
| `--remote` | `-R` | Discover Gitea login from this git remote name |
| `--login` | `-l` | Use a specific Gitea login by name |

## Output Flags

Available on list/detail commands:

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--output` | `-o` | Format: `simple`, `table`, `csv`, `tsv`, `yaml`, `json` | `table` |
| `--fields` | `-f` | Comma-separated list of fields to display | varies |

## Pagination Flags

Available on list commands:

| Flag | Alias | Description | Default |
|------|-------|-------------|---------|
| `--page` | `-p` | Page number | 1 |
| `--limit` | `--lm` | Items per page | 30 |

---

## Issues (`tea issues` / `tea issue` / `tea i`)

Lists issues when called without a subcommand. An issue index shows that issue in detail.

### `tea issues list` (alias: `ls`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--state` | | `open`, `closed`, `all` (default: `open`) |
| `--kind` | `-K` | `issues`, `pulls`, `all` (use this to apply issue-list filters to PRs) |
| `--keyword` | `-k` | Search string |
| `--labels` | `-L` | Comma-separated label names |
| `--milestones` | `-m` | Comma-separated milestone names |
| `--author` | `-A` | Filter by author username |
| `--assignee` | `-a` | Filter by assignee username |
| `--mentions` | `-M` | Filter by mentioned username |
| `--owner` | `--org` | Filter by owner/org |
| `--from` | `-F` | Activity after date |
| `--until` | `-u` | Activity before date |
| `--comments` | | Display comments (on the parent `tea issues` command) |

Issue fields: `index,state,kind,author,author-id,url,title,body,created,updated,deadline,assignees,milestone,labels,comments,owner,repo`.

### `tea issues create` (alias: `c`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | Issue title (required) |
| `--description` | `-d` | Issue body |
| `--assignees` | `-a` | Comma-separated usernames |
| `--labels` | `-L` | Comma-separated labels |
| `--milestone` | `-m` | Milestone name |
| `--deadline` | `-D` | Deadline timestamp |
| `--referenced-version` | `-v` | Commit hash or tag name |

### `tea issues edit` (alias: `e`)

Takes one or more issue indices. Empty string unsets a property (`--milestone ""`).

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | New title |
| `--description` | `-d` | New description |
| `--set-assignees` | | Replace all assignees (takes precedence) |
| `--add-assignees` | `-a` | Add assignees |
| `--remove-assignees` | | Remove assignees |
| `--add-labels` | `-L` | Add labels (takes precedence over remove) |
| `--remove-labels` | | Remove labels |
| `--milestone` | `-m` | Change milestone (empty string to unset) |
| `--deadline` | `-D` | Change deadline |
| `--referenced-version` | `-v` | Commit hash or tag name |

### `tea issues close`

Takes one or more issue indices.

### `tea issues reopen` (alias: `open`)

Takes one or more issue indices.

---

## Comments (`tea comments` / `tea comment` / `tea c`)

`tea comment <index> [<body>]` still adds a comment (historical shorthand).

### `tea comments add` (alias: `a`)

`tea comments add <issue / pr index> [<comment body>]`

| Flag | Alias | Description |
|------|-------|-------------|
| `--description` | `-d` | Comment body (alternative to the positional argument) |

Body can also come from stdin or `$EDITOR`.

### `tea comments list` (alias: `ls`)

`tea comments list <issue / pr index>`

IDs in this output are what `edit` and `delete` accept.

### `tea comments edit` (alias: `e`)

`tea comments edit <comment id> [<new body>]`

| Flag | Alias | Description |
|------|-------|-------------|
| `--description` | `-d` | New body |

### `tea comments delete` (alias: `rm`)

`tea comments delete <comment id> [<comment id>...]`

---

## Pull Requests (`tea pulls` / `tea pull` / `tea pr`)

Lists PRs when called without a subcommand. A PR index shows that PR in detail (`tea pulls 15`).

### `tea pulls list` (alias: `ls`)

Does **not** accept issue-list filters (`--labels`, `--keyword`, `--assignee`, etc.). Filter PRs with `tea issues ls --kind pulls` plus those flags.

| Flag | Description |
|------|-------------|
| `--state` | `open`, `closed`, `all` (default: `open`) |

PR fields: `index,state,author,author-id,url,title,body,mergeable,base,base-commit,head,diff,patch,created,updated,deadline,assignees,milestone,labels,comments,ci`.

### `tea pulls create` (alias: `c`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | PR title (required) |
| `--description` | `-d` | PR body |
| `--base` | `-b` | Target branch (default: repo default branch) |
| `--head` | | Source branch (`user:branch` for fork PRs) |
| `--draft` | | Create as draft (prepends `WIP: ` to the title) |
| `--agit` | | Create an AGit-flow pull request |
| `--topic` | | Topic name for AGit-flow PRs |
| `--allow-maintainer-edits` | `--edits` | Allow maintainers to push to the head branch |
| `--assignees` | `-a` | Comma-separated usernames |
| `--labels` | `-L` | Comma-separated labels |
| `--milestone` | `-m` | Milestone name |
| `--deadline` | `-D` | Deadline |
| `--referenced-version` | `-v` | Commit hash or tag name |

### `tea pulls edit` (alias: `e`)

Takes one or more PR indices. Same issue-style assignee/label/title/description/milestone/deadline/referenced-version flags as `tea issues edit`, plus:

| Flag | Alias | Description |
|------|-------|-------------|
| `--add-reviewers` | `-r` | Request review from these usernames |
| `--remove-reviewers` | | Remove reviewers |
| `--draft` | | Mark draft by prepending `WIP: ` (idempotent) |
| `--ready` | | Mark ready by stripping a leading `WIP: ` or `[WIP]` prefix |

`--add-reviewers` aliases `-r`, which is also `--repo` on most commands. Use the long flag.

### `tea pulls checkout` (alias: `co`)

Takes PR index.

| Flag | Alias | Description |
|------|-------|-------------|
| `--branch` | `-b` | Boolean: create a local branch if it does not exist yet (does not take a name) |

### `tea pulls review`

Interactive review. Takes PR index.

### `tea pulls approve` (aliases: `lgtm`, `a`)

`tea pulls approve <pull index> [<comment>]`

### `tea pulls reject`

`tea pulls reject <pull index> <reason>` — reason is required.

### `tea pulls merge` (alias: `m`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--style` | `-s` | `merge`, `rebase`, `squash`, `rebase-merge` (default: `merge`) |
| `--title` | `-t` | Merge commit title |
| `--message` | `-m` | Merge commit message |

### `tea pulls clean`

Delete local and remote branches for a merged/closed PR. Takes PR index.

| Flag | Description |
|------|-------------|
| `--ignore-sha` | Find the local branch by name instead of commit hash |

### `tea pulls close` / `tea pulls reopen`

Takes one or more PR indices.

### `tea pulls review-comments` (alias: `rc`)

`tea pulls review-comments <pull index>`

Fields: `id,body,reviewer,path,line,resolver,created,updated,url`.

### `tea pulls reply`

`tea pulls reply <pull index> <comment id> [<reply>]`

### `tea pulls resolve` / `tea pulls unresolve`

`tea pulls resolve <comment id>` / `tea pulls unresolve <comment id>`

---

## Repositories (`tea repos` / `tea repo`)

### `tea repos list` (alias: `ls`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--type` | `-T` | `fork`, `mirror`, `source` |
| `--owner` | `-O` | List repos of this owner |
| `--watched` | `-w` | List watched repos instead |
| `--starred` | `-s` | List starred repos instead |

Repo fields: `description,forks,id,name,owner,stars,ssh,updated,url,permission,type`.

### `tea repos search` (alias: `s`)

Takes an optional search query.

| Flag | Alias | Description |
|------|-------|-------------|
| `--topic` | `-t` | Search in repo topics instead of name |
| `--type` | `-T` | `fork`, `mirror`, `source` |
| `--owner` | `-O` | Filter by owner |
| `--private` | | Filter private repos (`true`/`false`) |
| `--archived` | | Filter archived repos (`true`/`false`) |

### `tea repos create` (alias: `c`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--name` | | Repository name (required) |
| `--owner` | `-O` | Owner (user or org, default: authenticated user) |
| `--private` | | Make repository private |
| `--description` | `--desc` | Repository description |
| `--init` | | Initialize repository |
| `--labels` | | Label set to add |
| `--gitignores` | `--git` | Gitignore templates (requires `--init`) |
| `--license` | | License template (requires `--init`) |
| `--readme` | | Readme template (requires `--init`) |
| `--branch` | | Default branch name (requires `--init`) |
| `--template` | | Make repo a template |
| `--trustmodel` | | `committer`, `collaborator`, `collaborator+committer` |
| `--object-format` | | `sha1`, `sha256` |

### `tea repos create-from-template` (alias: `ct`)

Does **not** share `create`'s `--init` / `--gitignores` / `--license` flags.

| Flag | Alias | Description |
|------|-------|-------------|
| `--template` | `-t` | Source template to copy from (required) |
| `--name` | `-n` | New repo name (required) |
| `--owner` | `-O` | Owner |
| `--private` | | Make new repo private |
| `--description` | `--desc` | Description |
| `--content` | | Copy git content |
| `--githooks` | | Copy git hooks |
| `--avatar` | | Copy avatar |
| `--labels` | | Copy labels |
| `--topics` | | Copy topics |
| `--webhooks` | | Copy webhooks |

### `tea repos fork` (alias: `f`)

Does **not** take a positional slug. Use `--repo owner/repo`. Extra arguments are ignored and tea forks the current repo instead.

| Flag | Alias | Description |
|------|-------|-------------|
| `--owner` | `-O` | Fork to this owner (default: authenticated user) |

### `tea repos edit` (alias: `e`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--name` | | New repository name |
| `--description` | `--desc` | New description |
| `--website` | | Website URL |
| `--private` | | `true`/`false` |
| `--template` | | `true`/`false` |
| `--archived` | | `true`/`false` |
| `--default-branch` | | Default branch |

### `tea repos migrate` (alias: `m`)

| Flag | Description |
|------|-------------|
| `--name` | Repository name |
| `--owner` | Owner |
| `--clone-url` | Source clone URL |
| `--service` | `git`, `gitea`, `gitlab`, `gogs` (no `github` value; use `git`) |
| `--mirror` | Create as mirror |
| `--private` | Make private |
| `--template` | Make the repository a template |
| `--wiki` | Copy wiki |
| `--issues` | Copy issues |
| `--labels` | Copy labels |
| `--pull-requests` | Copy PRs |
| `--releases` | Copy releases |
| `--milestones` | Copy milestones |
| `--lfs` | Copy LFS objects |
| `--lfs-endpoint` | LFS endpoint URL |
| `--auth-user` | Auth username for source |
| `--auth-password` | Auth password for source |
| `--auth-token` | Auth token for source |
| `--mirror-interval` | Mirror sync interval (e.g., `8h`) |

### `tea repos delete` (alias: `rm`)

Destructive. Does **not** accept `--repo`.

| Flag | Alias | Description |
|------|-------|-------------|
| `--name` | | Repository name |
| `--owner` | `-O` | Owner |
| `--force` | `-f` | Skip confirmation prompt |

---

## Releases (`tea releases` / `tea release` / `tea r`)

Create/edit/delete/assets take a **release tag**, not a numeric ID.

### `tea releases list` (alias: `ls`)

Standard output and pagination flags.

### `tea releases create` (alias: `c`)

Optional positional `<tag>` in addition to `--tag`.

| Flag | Alias | Description |
|------|-------|-------------|
| `--tag` | | Tag name (creates if it does not exist) |
| `--target` | | Target branch/commit (default: default branch) |
| `--title` | `-t` | Release title |
| `--note` | `-n` | Release notes |
| `--note-file` | `-f` | Release notes from file (overrides `--note`) |
| `--draft` | `-d` | Mark as draft |
| `--prerelease` | `-p` | Mark as pre-release |
| `--asset` | `-a` | File attachment (repeatable) |

### `tea releases edit` (alias: `e`)

`tea releases edit <release tag> [<release tag>...]`

| Flag | Alias | Description |
|------|-------|-------------|
| `--tag` | | Change tag |
| `--target` | | Change target |
| `--title` | `-t` | Change title |
| `--note` | `-n` | Change notes |
| `--draft` | `-d` | Mark as draft (`True`/`false`) |
| `--prerelease` | `-p` | Mark as pre-release (`True`/`false`) |

### `tea releases delete` (alias: `rm`)

`tea releases delete <release tag> [<release tag>...]`

| Flag | Alias | Description |
|------|-------|-------------|
| `--confirm` | `-y` | Confirm deletion (required) |
| `--delete-tag` | | Also delete the git tag |

### `tea releases assets` (alias: `asset`, `a`)

- `tea releases assets ls <release-tag>`
- `tea releases assets create <release-tag> <file> [<file>...]`
- `tea releases assets delete <release-tag> <attachment name> [<name>...]` — `--confirm` / `-y` required

---

## Labels (`tea labels` / `tea label`)

### `tea labels list` (alias: `ls`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--save` | `-s` | Save all labels to a file |
| `--org` | | List organization labels |
| `--exclude-org` | | Exclude organization labels |

### `tea labels create` (alias: `c`)

| Flag | Description |
|------|-------------|
| `--name` | Label name (required) |
| `--color` | Hex color (e.g., `#ff0000`) |
| `--description` | Label description |
| `--file` | Create from a label file |

### `tea labels update`

No positional ID. Use `--id`.

| Flag | Description |
|------|-------------|
| `--id` | Label id |
| `--name` | Label name |
| `--color` | Hex color |
| `--description` | Label description |

### `tea labels delete` (alias: `rm`)

| Flag | Description |
|------|-------------|
| `--id` | Label id |

---

## Milestones (`tea milestones` / `tea milestone` / `tea ms`)

Subcommands take the **milestone title**, not a numeric ID.

### `tea milestones list` (alias: `ls`)

| Flag | Description |
|------|-------------|
| `--state` | `open`, `closed`, `all` (default: `open`) |

Fields: `title,state,items_open,items_closed,items,duedate,description,created,updated,closed,id`.

### `tea milestones create` (alias: `c`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | Milestone title (required) |
| `--description` | `-d` | Description |
| `--deadline` | `--expires`, `-x` | Deadline |
| `--state` | | `open` (default) or `closed` |

### `tea milestones close`

`tea milestones close <milestone name> [<name>...]`

| Flag | Alias | Description |
|------|-------|-------------|
| `--force` | `-f` | Delete the milestone instead of closing it |

### `tea milestones reopen` (alias: `open`)

`tea milestones reopen <milestone name> [<name>...]`

### `tea milestones delete` (alias: `rm`)

`tea milestones delete <milestone name>`

### `tea milestones issues` (alias: `i`)

`tea milestones issues <milestone name>` lists issues/PRs in that milestone.

| Flag | Description |
|------|-------------|
| `--state` | `open`, `closed`, `all` |
| `--kind` | `issue` or `pull` |

Subcommands:

- `tea milestones issues add <milestone name> <issue/pull index>`
- `tea milestones issues remove <milestone name> <issue/pull index>`

---

## Time Tracking (`tea times` / `tea time` / `tea t`)

### `tea times list` (alias: `ls`)

`tea times list [username | #issue]`

Quote the `#` in shells: `tea times ls '#42'`. A bare number is treated as a username.

| Flag | Alias | Description |
|------|-------|-------------|
| `--from` | `-f` | Times after this date |
| `--until` | `-u` | Times before this date |
| `--total` | `-t` | Print the total duration |
| `--mine` | `-m` | Your times across all repositories |

Fields: `id,created,repo,issue,user,duration`.

### `tea times add` (alias: `a`)

`tea times add <issue> <duration>`

Duration format: `1h30m`, `2h`, `45m`. Issue index is a bare number (no `#`).

### `tea times delete` (alias: `rm`)

`tea times delete <issue> <time ID>`

### `tea times reset`

`tea times reset <issue>` — reset all tracked time on an issue.

---

## Actions (`tea actions` / `tea action`)

### Secrets (`tea actions secrets` / `secret`)

- `tea actions secrets ls`
- `tea actions secrets create` (aliases: `add`, `set`) `<name> [value]`
  - `--file` — read value from file
  - `--stdin` — read value from stdin
- `tea actions secrets delete` (aliases: `remove`, `rm`) `<name>`
  - `--confirm` / `-y`

### Variables (`tea actions variables` / `variable` / `vars` / `var`)

- `tea actions variables ls` — `--name` shows one variable
- `tea actions variables set` (aliases: `create`, `update`) `<name> [value]`
  - `--file` / `--stdin`
- `tea actions variables delete` (aliases: `remove`, `rm`) `<name>`
  - `--confirm` / `-y`

### Runs (`tea actions runs` / `run`)

- `tea actions runs ls`
  - `--status` — `success`, `failure`, `pending`, `queued`, `in_progress`, `skipped`, `canceled`
  - `--branch` — branch name
  - `--event` — event type (`push`, `pull_request`, …)
  - `--actor` — username who triggered the run
  - `--since` / `--until` — time window (`24h` or `2024-01-01`)
- `tea actions runs view` (aliases: `show`, `get`) `<run-id>`
  - `--jobs` — show jobs table
- `tea actions runs logs` (alias: `log`) `<run-id>`
  - `--job` — specific job ID
  - `--follow` / `-f` — follow log output
- `tea actions runs delete` (aliases: `remove`, `rm`, `cancel`) `<run-id>`
  - `--confirm` / `-y`

### Workflows (`tea actions workflows` / `workflow`)

- `tea actions workflows ls`
- `tea actions workflows view` (aliases: `show`, `get`) `<workflow-id>`
- `tea actions workflows dispatch` (aliases: `trigger`, `run`) `<workflow-id>`
  - `--ref` — branch or tag (also claims `-r`; do not use `-r`)
  - `--input` / `-i` — `key=value` (repeatable)
  - `--follow` / `-f` — follow logs after dispatch
- `tea actions workflows enable <workflow-id>`
- `tea actions workflows disable <workflow-id>` — `--confirm` / `-y`

---

## Webhooks (`tea webhooks` / `tea webhook` / `tea hooks` / `tea hook`)

### `tea webhooks list` (alias: `ls`)

Standard output flags.

### `tea webhooks create` (alias: `c`)

`tea webhooks create <webhook-url>` — URL is positional. There is no `--url` on create.

| Flag | Description |
|------|-------------|
| `--type` | `gitea`, `gogs`, `slack`, `discord`, `dingtalk`, `telegram`, `msteams`, `feishu`, `wechatwork`, `packagist` (default: `gitea`) |
| `--secret` | Webhook secret |
| `--events` | Comma-separated events (default: `push`) |
| `--active` | Webhook is active |
| `--branch-filter` | Branch filter for push events |
| `--authorization-header` | Authorization header value |

### `tea webhooks update` (aliases: `edit`, `u`)

Takes webhook ID. This is the command that has `--url`.

| Flag | Description |
|------|-------------|
| `--url` | New webhook URL |
| `--secret` | New secret |
| `--events` | New events |
| `--active` | Mark active |
| `--inactive` | Mark inactive |
| `--branch-filter` | Branch filter |
| `--authorization-header` | Authorization header |

### `tea webhooks delete` (alias: `rm`)

Takes webhook ID. `--confirm` / `-y` skips the prompt (pass this from agents).

---

## Organizations (`tea organizations` / `tea organization` / `tea org`)

There is no `orgs` alias.

### `tea org list` (alias: `ls`)

Standard output flags.

### `tea org create` (alias: `c`)

`tea org create <organization name>` — slug is positional.

| Flag | Alias | Description |
|------|-------|-------------|
| `--full-name` | `-n` | Display name (not the slug) |
| `--description` | `-d` | Description |
| `--website` | `-w` | Website URL |
| `--location` | `-L` | Location |
| `--visibility` | `-v` | `public` (default), `private`, `limited` |
| `--repo-admins-can-change-team-access` | | Allow repo admins to change team access |

### `tea org delete` (alias: `rm`)

Takes org name. Destructive.

---

## Wiki (`tea wiki`)

### `tea wiki list` (alias: `ls`)

Fields: `title,path,url,sha,author,updated,message`.

### `tea wiki view`

`tea wiki view <page>`

### `tea wiki revisions` (alias: `history`)

`tea wiki revisions <page>`

Fields: `sha,message,author,date`.

### `tea wiki create` (alias: `c`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | Page title |
| `--content` | `-c` | Page content |
| `--message` | `-m` | Commit message |

### `tea wiki edit` (alias: `e`)

`tea wiki edit <page>` — same `--title` / `--content` / `--message` flags as create.

### `tea wiki delete` (alias: `rm`)

`tea wiki delete <page>` — `--confirm` / `-y` skips the prompt.

---

## Branches (`tea branches` / `tea branch` / `tea b`)

Lists branches when called without a subcommand. A branch name shows that branch in detail.

Fields: `name,protected,user-can-merge,user-can-push,protection`.

### `tea branches list` (alias: `ls`)

Standard output flags.

### `tea branches protect` (alias: `P`)

`tea branches protect <branch>`

### `tea branches unprotect` (alias: `U`)

`tea branches unprotect <branch>`

### `tea branches rename` (alias: `rn`)

`tea branches rename <old_branch_name> <new_branch_name>`

---

## Notifications (`tea notifications` / `tea notification` / `tea n`)

### `tea notifications list` (alias: `ls`)

| Flag | Alias | Description |
|------|-------|-------------|
| `--types` | `-t` | `issue`, `pull`, `repository`, `commit` (comma-separated) |
| `--states` | `-s` | `pinned`, `unread`, `read` (default: `unread,pinned`) |
| `--mine` | `-m` | Show across all repos (not just current) |

### `tea notifications read` (alias: `r`)

`tea notifications read [all | <notification id>]`

### `tea notifications unread` (alias: `u`)

Same shape as `read`.

### `tea notifications pin` (alias: `p`) / `tea notifications unpin`

Takes a notification ID, or applies to the filtered set.

---

## SSH Keys (`tea ssh-keys` / `tea ssh-key`)

### `tea ssh-keys list` (alias: `ls`)

Standard output flags. No repo context; keys belong to the logged-in user.

### `tea ssh-keys add`

`tea ssh-keys add <key-file>`

| Flag | Alias | Description |
|------|-------|-------------|
| `--title` | `-t` | Title (defaults to filename without extension) |

### `tea ssh-keys delete` (alias: `rm`)

`tea ssh-keys delete <key-id>` — `--confirm` / `-y` required.

---

## Logins (`tea logins` / `tea login`)

### `tea logins list` (alias: `ls`)

Standard output flags.

### `tea logins add`

See `references/authentication.md` for the complete flag reference, including `--git-credentials`.

### `tea logins edit` (alias: `e`)

Edit an existing login configuration.

### `tea logins delete` (alias: `rm`)

Takes login name.

### `tea logins default`

Get or set the default login. Takes optional login name.

### `tea logins helper` (alias: `git-credential`)

Git credential helper for stored tokens.

- `tea logins helper setup` — register tea in `~/.gitconfig` for every login
- `tea logins helper get` — git credential protocol
- `tea logins helper store` (alias: `erase`) — no-op required by the protocol

### `tea logins oauth-refresh`

`tea logins oauth-refresh [<login name>]`

### `tea logout`

`tea logout <login name>` — name is required.

---

## Other Commands

| Command | Alias | Description |
|---------|-------|-------------|
| `tea clone <repo> [dir]` | `C` | Clone a repository |
| `tea open [target]` | `o` | Open repo home, or `tea open 42` / `tea open pulls` (also `issues`, `wiki`, `settings`, `labels`, `milestones`) |
| `tea whoami` | | Show current logged-in user |
| `tea admin users ls` | | List all users (admin only) |
| `tea api <endpoint>` | | Make authenticated API requests |

### `tea api` flags

| Flag | Alias | Description |
|------|-------|-------------|
| `--method` | `-X` | HTTP method: `GET`, `POST`, `PUT`, `PATCH`, `DELETE` (default `GET`; becomes `POST` if a body is given) |
| `--field` | `-f` | String field: `key=value` (repeatable) |
| `--Field` | `-F` | Typed field: `key=value`, `@file`, `@-` (stdin) |
| `--header` | `-H` | Custom header: `key:value` (repeatable) |
| `--data` | `-d` | Raw JSON body (`@file` or `@-`). Cannot be combined with `-f`/`-F` |
| `--include` | `-i` | Include HTTP status and headers (written to stderr) |
| `--output` | `-o` | Write response to file (`-` for stdout) |

### `tea admin users`

- `tea admin users ls` — list users
- `tea admin users create` (aliases: `add`, `new`) — `--username` / `-u` and `--email` / `-e` required; `--password` / `--password-file` / `--password-stdin`; `--admin`; `--restricted`; `--prohibit-login`; `--no-must-change-password`; `--visibility`; `--full-name`
- `tea admin users edit` (aliases: `update`, `e`, `u`) `<username>` — password/email/profile flags plus `--admin` / `--no-admin`, `--active` / `--inactive`, `--allow-login` / `--prohibit-login`, and permission toggles
- `tea admin users delete` (aliases: `rm`, `remove`) `<username>` — `--confirm` / `-y`

### `tea clone` flags

| Flag | Alias | Description |
|------|-------|-------------|
| `--depth` | `-d` | Number of commits to fetch (0 = all) |
| `--login` | `-l` | Use specific login instance |

Supports slug formats: `owner/repo`, `repo`, `gitea.com/owner/repo`, `git@gitea.com:owner/repo`, `https://gitea.com/owner/repo`, `ssh://gitea.com:22/owner/repo`.
