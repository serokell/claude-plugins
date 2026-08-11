<!--
   - SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
   -
   - SPDX-License-Identifier: CC0-1.0
   -->

# Setting up Claude for Serokell work

A one-time setup guide for Serokell engineers. Do this before working on any project with Claude Code.

## Prerequisites

Install Claude Code from the [official page](https://claude.com/product/claude-code).

## 1. Connect Notion

Claude Code can read and write Notion pages when the Notion integration is enabled.

In a Claude Code session, type `/mcp`. From the list, choose **Notion**. A browser window opens. Authorize access and return to the terminal. Done.

## 2. Connect Google Drive

Claude Code can read Google Drive files when the Google Drive integration is enabled.

In a Claude Code session, type `/mcp`. From the list, choose **Google Drive**. A browser window opens confirming what Claude should be able to access — select all, then authorize and return to the terminal. Done.

## 3. Connect YouTrack

Claude uses a permanent token to create and update issues in `issues.serokell.io`.

In YouTrack, click your name in the bottom-left corner, then go to **Profile → Account Security → Tokens**. Create a permanent token with the `YouTrack` scope. Copy the token (it starts with `perm:`).

Add it to your user-level Claude environment file:

```bash
echo "YOUTRACK_TOKEN=perm:..." >> ~/.claude/.env
chmod 600 ~/.claude/.env
```

Claude Code loads `~/.claude/.env` on startup, so the token is available in every session.

**Never paste a secret into a Claude prompt.** If a secret appears in the agent context, treat it as compromised and rotate it immediately. Store credentials in files, not in messages.

## 4. Install the Serokell plugin

The Serokell plugin bundle ships skills for everything from bootstrapping a repo to reviewing a PR. It is hosted at `serokell/claude-plugins` on GitHub.

In a Claude Code session, add the marketplace and install the global plugin:

```
/plugin marketplace add serokell/claude-plugins
/plugin install serokell-global@serokell
```

When prompted for a scope, choose **User** — this makes the plugin available in every project, not just the one you're currently in.

Equivalently, add the marketplace and enable the plugin directly in `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "serokell": {
      "source": { "source": "github", "repo": "serokell/claude-plugins" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": ["serokell-global@serokell"]
}
```

This gives you the workflow skills (`committing-work`, `youtrack-issues`, `pull-requests`, `code-review`) in every project.

For projects that use metatemplates, the full skill set is already wired up via the project-level `.claude/settings.json`.

## 5. Configure commit signing

Claude Code commits on your behalf. All commits at Serokell must be GPG-signed.

Make sure signing is on:

```bash
git config --global commit.gpgsign true
```

If GPG prompts for a passphrase via a Curses dialog, or SSH does via stdin,
neither works from a terminal-only session — Claude Code can't answer
either kind of prompt. See [gpg-pinentry.md](gpg-pinentry.md) and
[ssh-passphrase.md](ssh-passphrase.md) for the fixes, both optional.
