# claude-switch

Toggle Claude Code between two accounts (personal/work) without re-running `/login`.

## Usage

```
claude-switch          # toggle to the other saved account
claude-switch status   # show active account and saved profiles
```

## How it works

Claude Code keeps account state in two places:

- `~/.claude/.credentials.json` — OAuth tokens
- `~/.claude.json` — the `oauthAccount` and `userID` keys

On every toggle the script first saves the live credentials and identity of the
current account into `~/.claude/profiles/<email>/` (so token refreshes are never
lost), then restores the other profile. Only those two account keys in
`~/.claude.json` are modified; settings, history, and plugins are shared between
accounts. All writes are atomic (temp file + `mv`), profile files are `chmod 600`,
and a rolling backup is kept at `~/.claude.json.account-switch.bak`.

## First-time setup

1. `claude-switch` — saves the currently logged-in account and tells you what's next.
2. In Claude Code, run `/login` and sign into your **other** account.
3. From now on, `claude-switch` toggles between the two.

Don't switch while a Claude Code session is open — a live session can rewrite the
credentials file and mix up the accounts. The script warns if it detects one.

If a token expires while its profile is inactive, Claude Code refreshes it on next
use. If the refresh token was revoked, just `/login` again on that account.

## Install

```
chmod +x claude-switch
ln -s "$PWD/claude-switch" ~/.local/bin/claude-switch
```

Requires `bash` and `jq`.
# claude-switch
