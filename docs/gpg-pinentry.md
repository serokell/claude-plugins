<!--
   - SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
   -
   - SPDX-License-Identifier: CC0-1.0
   -->

# GPG commit-signing pinentry

If GPG prompts for a passphrase via a Curses dialog (which does not work in a terminal-only session), switch to a graphical pinentry. For example on KDE:

```bash
echo "pinentry-program /usr/bin/pinentry-qt" >> ~/.gnupg/gpg-agent.conf
gpg-connect-agent reloadagent /bye
```

This switches pinentry to graphical everywhere, including plain terminal use
outside Claude Code. **Optional, if you'd rather keep Curses for normal
terminal work** and only switch while Claude Code is running: point
`pinentry-program` at a small wrapper that checks a sentinel file instead,
and toggle that file from Claude Code's `SessionStart`/`SessionEnd` hooks.

`~/.local/bin/pinentry-claude-wrapper` (`chmod +x`):

```bash
#!/bin/bash
if [ -f "/run/user/$(id -u)/claude-pinentry-active" ]; then
    exec /usr/bin/pinentry-qt "$@"
else
    exec /usr/bin/pinentry-curses "$@"
fi
```

```bash
echo "pinentry-program $HOME/.local/bin/pinentry-claude-wrapper" >> ~/.gnupg/gpg-agent.conf
gpg-connect-agent reloadagent /bye
```

In `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [{ "hooks": [{ "type": "command", "command": "touch /run/user/1000/claude-pinentry-active" }] }],
    "SessionEnd": [{ "hooks": [{ "type": "command", "command": "rm -f /run/user/1000/claude-pinentry-active" }] }]
  }
}
```

(Use your actual UID in place of `1000` if it differs — `id -u`.)

See also [ssh-passphrase.md](ssh-passphrase.md) for the equivalent SSH problem.
