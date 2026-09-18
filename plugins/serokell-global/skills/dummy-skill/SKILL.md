---
# SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
# SPDX-License-Identifier: CC0-1.0
name: dummy-skill
description: Use when testing that skills in this marketplace are discovered and loaded correctly. Triggers on phrases like "test the dummy skill", "is the dummy skill loaded", "run the dummy skill check".
---

# Dummy Skill

This is a placeholder skill used to verify that skills added to the
`serokell-global` plugin are correctly packaged, discovered, and loaded
by Claude Code. It has no real functionality.

## When invoked

Reply with exactly: `dummy skill loaded`.

This skill is safe to remove once the plugin/marketplace mechanism has
been verified.
