<!--
   - SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
   -
   - SPDX-License-Identifier: CC0-1.0
   -->

# SSH key passphrase prompts

An SSH key with a passphrase (e.g. `ssh-add`, or a `git push` that needs to
unlock the key) normally prompts on stdin — which blocks the same way as
GPG's Curses pinentry (see [gpg-pinentry.md](gpg-pinentry.md)), and which
Claude Code cannot answer either. The fix is a graphical askpass instead of
stdin, forced on even though Claude Code's shell has a tty (by default
`ssh`/`ssh-add` only use `SSH_ASKPASS` when there is no controlling terminal
at all). Set it directly in the `env` block of `~/.claude/settings.json` so
every Claude Code session has it (swap `ksshaskpass` below for your own
desktop's askpass helper if you're not on KDE):

```json
{
  "env": {
    "SSH_ASKPASS": "/usr/bin/ksshaskpass",
    "SSH_ASKPASS_REQUIRE": "force"
  }
}
```

Recommended alongside it, otherwise the dialog reappears on every single
push and pull: cache the unlocked key in a persistent `ssh-agent` instead of
unlocking it fresh each time. Many distros (Arch included) *ship* a systemd
user `ssh-agent.service` + socket (part of the `openssh` package), but
shipping the unit isn't the same as enabling it — check with
`systemctl --user is-enabled ssh-agent.socket`, and if it isn't:

```bash
systemctl --user enable --now ssh-agent.socket
```

That gets you a persistent agent with `SSH_AUTH_SOCK` pointed at
`$XDG_RUNTIME_DIR/ssh-agent.socket`. What's still usually missing is telling
`ssh` to actually *use* it: add

```
Host *
    AddKeysToAgent yes
```

to `~/.ssh/config`. The first connection each login unlocks the key (via
the askpass dialog above) and caches it in the agent; every connection after
that reuses the cached identity — no repeated prompts.
