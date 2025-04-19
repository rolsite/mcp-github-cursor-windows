# mcp-github-cursor-windows
A GitHub repository for testing and development with Cursor IDE integration

## English Version (Default)

This project demonstrates how to integrate the GitHub MCP Server with Cursor IDE on Windows.

- [Setup instructions](#setup)
- [How MCP works](#how-mcp-works)
- [Usage: Git vs MCP](#usage-git-vs-mcp)
- [Natural Language Commands](#natural-language-commands)
- [Common issues](#common-issues)
- [Cursor Rules for MCP](#cursor-rules)

## [Versão em Português Brasileiro](pt-br-README.md)

---

## Setup

1. **Install Go, Git, and Cursor IDE**
2. **Clone and build the github-mcp-server**
3. **Create a GitHub personal access token**
4. **Configure MCP in Cursor (global or per project)**
5. **Add your token to the configuration**

## How MCP works

- MCP (Model Context Protocol) allows Cursor to interact directly with GitHub, bypassing local git commands for advanced GitHub features.
- All MCP operations are executed via natural language commands and are reflected immediately on the remote repository.
- Local Git is still used for all standard version control operations.

## Usage: Git vs MCP

### What the local Git does (standard workflow)
Use the integrated Git for all standard version control operations:
- Local commit (`git commit`)
- Branch creation and switching (`git branch`, `git checkout`)
- Merge, rebase, cherry-pick (`git merge`, `git rebase`, `git cherry-pick`)
- Push and pull (`git push`, `git pull`)
- Conflict resolution
- History and log (`git log`)
- Stash, reset, checkout, etc.

**Example command:**
- `git commit -m "my message"`
- `git push`
- `git checkout feature/login`

> These commands are run in your terminal or via the Source Control panel in your editor.

### What the MCP does (advanced GitHub integration)
Use MCP for tasks not covered by local Git, such as:
- Creating, listing, or managing GitHub issues directly from the editor
- Creating, listing, or managing pull requests (PRs) directly from the editor
- Commenting, reviewing, and approving PRs in-editor
- Managing labels, milestones, and assignees for issues/PRs
- Viewing and interacting with security/code scanning/secret scanning alerts
- Administrative repository operations (fork, create repo, configure integrations)
- Automating GitHub Actions workflows

**Example natural language commands:**
- "Create a new issue titled 'Bug in login page'"
- "List all open pull requests"
- "Approve pull request #42"
- "Assign the label 'urgent' to issue #10"
- "Fork this repository"

> These commands are written in natural language and executed by the MCP integration in Cursor. The corresponding GitHub API operation is triggered automatically.

## Natural Language Commands

- MCP enables you to use natural language for advanced GitHub operations. See `.cursor/rules/mcp-commands.mdc` for English and `.cursor/rules/mcp-commands-ptbr.mdc` for Portuguese examples.
- For standard versioning, continue using Git commands as usual.

## Common Issues

- **Local files out of sync:** Always run `git fetch` and `git reset --hard origin/main` after MCP operations that change the remote repository.
- **Token errors:** Make sure your GitHub token is valid and has the correct scopes.
- **MCP server not found:** Check your configuration paths and that the server is built.

## Cursor Rules for MCP

- See `.cursor/rules/mcp-integration.mdc` for when to use Git vs MCP.
- See `.cursor/rules/mcp-commands.mdc` and `.cursor/rules/mcp-commands-ptbr.mdc` for natural language command examples.

---

This README is the default (English) version. For the original Portuguese instructions, see [pt-br-README.md](pt-br-README.md).
