<!--
   - SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
   -
   - SPDX-License-Identifier: MPL-2.0
   -->

# claude-plugins

> Claude Code plugin marketplace for Serokell engineers

## Background

This repo is a [Claude Code](https://claude.com/product/claude-code) plugin marketplace.
It ships the `serokell-global` plugin, which contains skills covering the full Serokell project lifecycle.

## Install

Add the marketplace and enable the plugin in `~/.claude/settings.json`:

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

For full setup instructions, see [docs/claude-setup.md](docs/claude-setup.md).

## Skills

| Skill | Description |
|---|---|
| `bootstrap-repo` | Apply metatemplates defaults to a new repo |
| `readme` | Write a Standard Readme |
| `reuse-headers` | Add SPDX copyright and license headers |
| `gitignore` | Set up `.gitignore` for Haskell and Nix |
| `license-choice` | Pick the right license for a Serokell project |
| `haskell-style` | Write Haskell following the Serokell style guide |
| `setup-ci` | Wire up GitHub Actions with `serokell/nix-templates` |
| `code-testing` | Write tests following Serokell's testing philosophy |
| `changelog` | Maintain a Keep a Changelog file |
| `committing-work` | Branch naming and commit message conventions |
| `pull-requests` | Open, review, and merge PRs |
| `code-review` | Structured checklist for reviewing PRs |
| `youtrack-issues` | File and manage YouTrack issues |
| `nix-binary-cache` | Set up the Serokell private Nix binary cache |
| `repository-settings` | Apply standard GitHub repository settings |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MPL-2.0](LICENSES/MPL-2.0.txt) © [Serokell](https://serokell.io)

Skills in `plugins/serokell-global/skills/` are licensed under [CC0-1.0](LICENSES/CC0-1.0.txt).
