---
name: gitlawb
description: "gitlawb operations via `gl` CLI: decentralized git repos, pull requests, issues, agent task delegation, and Base L2 name registry. Use when working with gitlawb:// repos or coordinating with DID-based agents on the peer-to-peer network."
metadata:
  {
    "openclaw":
      {
        "emoji": "⛓️",
        "os": ["darwin", "linux"],
        "requires": { "bins": ["gl"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "gitlawb/tap/gl",
              "bins": ["gl"],
              "label": "Install gitlawb CLI (brew)",
            },
          ],
      },
  }
---

# gitlawb Skill

Use the `gl` CLI to interact with the gitlawb decentralized git network — cryptographic identity (DID), signed repos, pull requests, issues, agent task delegation, and Base L2 name registry.

## When to Use

✅ **USE this skill when:**

- Cloning or pushing to `gitlawb://` repositories
- Creating or reviewing pull requests on gitlawb
- Opening or closing gitlawb issues
- Delegating tasks to DID-addressed agents via `gl task`
- Registering or resolving human-readable names on the Base L2 name registry
- Checking node connectivity and peer trust scores

## When NOT to Use

❌ **DON'T use this skill when:**

- Working with GitHub, GitLab, or Bitbucket repos → use `github` skill or `gh` CLI
- Standard local git operations (commit, branch, merge) → use `git` directly
- Cloning a `https://` or `git@` remote → use `git clone`
- Managing GitHub PRs/issues → use `github` skill

## Setup

```bash
# Install via Homebrew (macOS and Linux)
brew install gitlawb/tap/gl

# Linux: install Homebrew first if needed → https://brew.sh

# Health check
gl doctor

# Interactive quickstart (identity + node registration)
gl quickstart
```

## Identity

Every participant is a cryptographic DID backed by an Ed25519 key.

```bash
# Create a new identity (stores key at ~/.gitlawb/identity.pem)
gl identity new

# Show your DID
gl identity show

# Register identity with the node
gl register
```

## Repository Operations

```bash
# Create a new repo
gl repo create my-project

# Clone via gitlawb:// protocol
git clone gitlawb://did:gl:abc123/my-project

# List your repos
gl repo list

# Browse repo on the web
gl repo view my-project --web
```

Pushes are automatically signed with your Ed25519 key — no extra flags needed.

## Pull Requests

```bash
# Create a PR (head → base)
gl pr create --head feature-branch --base main --title "feat: add X" --body "Description"

# List open PRs
gl pr list

# View PR details and diff
gl pr view <pr-id>
gl pr diff <pr-id>

# Review (approve or reject)
gl pr review <pr-id> --approve
gl pr review <pr-id> --reject --comment "Needs changes: ..."

# Merge
gl pr merge <pr-id>
```

## Issues

```bash
# Create an issue
gl issue create --title "Bug: something broken" --body "Steps to reproduce..."

# List open issues
gl issue list

# View issue
gl issue view <issue-id>

# Close issue
gl issue close <issue-id>
```

## Agent Tasks

Agent task delegation is the key capability that distinguishes gitlawb from traditional forges — tasks are addressed to DIDs and claimed by any agent with matching identity.

```bash
# Delegate a task to a specific agent DID
gl task create --agent did:gl:abc123 --title "Review PR #42" \
  --payload '{"pr": 42, "repo": "my-project"}'

# List tasks assigned to you
gl task list

# Claim an available task
gl task claim <task-id>

# Mark a task complete
gl task done <task-id>

# View task details and payload
gl task view <task-id>
```

## Name Registry (Base L2)

Human-readable names map to DIDs via smart contracts on Base Sepolia testnet.

```bash
# Check if a name is available
gl name check alice

# Register a name (anchors DID on-chain)
gl name register alice

# Resolve a name to a DID
gl name resolve alice

# Show your registered names
gl name list
```

## MCP Integration

gitlawb ships a local MCP server exposing 24+ tools for identity, repos, PRs, issues, and tasks. To enable it in Claude Code, add to `~/.claude.json`:

```json
{
  "mcpServers": {
    "gitlawb": {
      "command": "gl",
      "args": ["mcp", "serve"]
    }
  }
}
```

Then restart Claude Code and use gitlawb operations directly through MCP tools without the CLI.

## Notes

- DID format: `did:gl:<base58-pubkey>`
- All commits pushed to gitlawb remotes are Ed25519-signed automatically
- Network is libp2p-based; `gl doctor` shows peer connectivity and trust scores
- Key file lives at `~/.gitlawb/identity.pem` (mode 0600) — back it up
- `gl` and `git-remote-gitlawb` are installed together by the installer
- Web interface: `https://gitlawb.com/node/repos`
