# SPDX-FileCopyrightText: 2026 Serokell <https://serokell.io>
# SPDX-License-Identifier: CC0-1.0

# Optional: Danger checks for PR/MR review

[serokell_danger](https://github.com/serokell/danger) is a
GitHub/GitLab-agnostic gem of [Danger](https://danger.systems/ruby/)
checks: commit style, license headers, MR/PR conventions,
merge-commit hygiene, trailing whitespace. It overlaps with some of
`setup-ci`'s common checks, but it needs a Ruby/Bundler toolchain, so
treat it as an optional addition, not part of the common-checks list.

If this repo wants commit-style or MR checks, or simple mechanical
code-style checks via a custom rule, see
[serokell/danger's README](https://github.com/serokell/danger#readme)
for the `Gemfile`/`Dangerfile` snippet (there's room to add your own
rules alongside the built-in ones), and
[metatemplates' docs/danger.md](https://github.com/serokell/metatemplates/blob/master/docs/danger.md)
for how to wire it into CI as its own job.

Unlike the nix-based CI setup-ci otherwise recommends, this job
doesn't need a self-hosted runner: `runs-on: ubuntu-latest` is enough,
since it's a plain Ruby/Bundler toolchain, not nix.

Tailor `commit_msg_prefix`/`title_prefix`'s `kinds` to this repo's
actual issue tracker (see `PROJECT.md`) if it uses only one of GitHub
Issues or YouTrack. The default accepts both.

## Handling the API token

This needs a `DANGER_GITHUB_API_TOKEN` (or `DANGER_GITLAB_API_TOKEN`
on GitLab) secret with permission to post PR/MR comments. Ask whether
it's already configured as a repo/group secret, rather than
requesting it directly.

Never ask the user to paste a token or any other secret into chat,
and never read a file that might contain one. If a secret ever shows
up in the conversation or in a file you read, treat it as compromised
and tell the user to rotate it right away.

If the secret isn't set up yet, disable the job with `if: ${{ false }}`
instead of commenting it out. Re-enabling later is just a one-line
edit.
