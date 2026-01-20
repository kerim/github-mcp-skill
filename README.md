# GitHub MCP Skill for Claude Code

A Claude Code skill that sets up per-project GitHub MCP configuration. This keeps your GitHub Personal Access Token (PAT) isolated to specific projects rather than granting global access.

## What It Does

When invoked, this skill:

1. Creates a `.mcp.json` file with GitHub MCP server configuration (token placeholder)
2. Adds `.mcp.json` to `.gitignore` to prevent accidental commits
3. Guides you through creating a fine-grained PAT with minimal permissions

## Installation

Copy the `SKILL.md` file to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/github-mcp
cp SKILL.md ~/.claude/skills/github-mcp/
```

Then restart Claude Code.

## Usage

In any project where you need GitHub access, invoke the skill:

```
/github-mcp
```

Or simply ask Claude to set up GitHub access - it will detect when this skill is needed.

## After Setup

1. Create a fine-grained PAT at https://github.com/settings/personal-access-tokens/new
   - Repository access: Select only the repositories you need
   - Permissions: Contents, Issues, Pull requests (Read and write)

2. Edit `.mcp.json` and replace `YOUR_TOKEN_HERE` with your PAT

3. Restart Claude Code

4. Verify with `claude mcp list`

## Why Per-Project?

- **Security**: Each project gets its own token with minimal permissions
- **Isolation**: Revoking one token doesn't affect other projects
- **Fine-grained control**: Limit access to specific repositories only

## Files Created

| File | Purpose |
|------|--------|
| `.mcp.json` | MCP server configuration for Claude Code (gitignored) |

## License

MIT
