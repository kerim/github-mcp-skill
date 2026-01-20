---
name: github-mcp
description: Set up project-level GitHub MCP when GitHub operations (PRs, issues, repo access) are needed and no project GitHub MCP is configured. Automatically triggers when Claude detects GitHub access is required.
---

# GitHub MCP Project Setup

This skill sets up per-project GitHub MCP configuration, keeping your GitHub Personal Access Token (PAT) isolated to specific projects rather than granting global access.

## When to Use

Use this skill when:
- You need to access GitHub (PRs, issues, repo content) from Claude Code
- The project doesn't have GitHub MCP configured yet
- You want to avoid using a global GitHub PAT

## Security Notice

**CRITICAL: Claude must NEVER read files that may contain tokens.**

- Never read `.mcp.json` after the user has added their token
- Never read `.env` files or any files that might contain secrets
- The user is responsible for adding their token directly to the file

## Instructions for Claude

When this skill is invoked:

### Step 1: Create .mcp.json with Placeholder

Write this file to the project root (creating or overwriting):

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

### Step 2: Update .gitignore

Add `.mcp.json` to `.gitignore` if not already present:

```bash
# Check and add to .gitignore
grep -q "^\.mcp\.json$" .gitignore 2>/dev/null || echo ".mcp.json" >> .gitignore
```

### Step 3: Guide the User

Tell the user:

> **Setup complete! Now you need to:**
>
> 1. **Create a Fine-Grained PAT** at: https://github.com/settings/personal-access-tokens/new
>    - Token name: "Claude Code - [project name]"
>    - Expiration: 90 days (or as needed)
>    - Repository access: "Only select repositories" → select just this repo
>    - Permissions:
>      - Contents: Read and write
>      - Issues: Read and write
>      - Pull requests: Read and write
>      - Metadata: Read-only (auto-selected)
>
> 2. **Edit `.mcp.json`** in your project root and replace `YOUR_TOKEN_HERE` with your PAT
>
> 3. **Restart Claude Code** (MCP only loads on startup)
>
> 4. **Verify** with: `claude mcp list`

## Alternative: CLI Method

If preferred, the user can run this command directly (replacing with their actual PAT):

```bash
claude mcp add-json github '{"type":"http","url":"https://api.githubcopilot.com/mcp","headers":{"Authorization":"Bearer YOUR_PAT_HERE"}}' --scope project
```

This stores the token directly in the project config.

## Security Notes

- `.mcp.json` is automatically added to `.gitignore` to prevent accidental commits
- Fine-grained PATs limit access to specific repositories only
- Project-level config means the token only works in this project directory
- Tokens can be revoked anytime at https://github.com/settings/tokens
- **Claude will never read the .mcp.json file after setup** to protect your token
