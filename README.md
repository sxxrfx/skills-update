# skills-update

One command that updates the agent skills and plugins on your machine, for every coding
agent you have installed, and can run itself every day.

```
$ skills-update
skills-update 2026-11-17T05:31:23.919Z
✓ Claude Code marketplaces: refreshed
✓ Claude Code plugin beautiful-rust@beautiful-rust (user): 1.6.0
– Codex marketplaces: none configured
– skills CLI: no tracked global skills
✓ gh skill: all up to date

Agents: Claude Code, Codex, OpenCode, Shared
0 updated, 3 current, 2 skipped, 0 failed
```

## What it updates

Agents that aren't installed are skipped.

| Agent | What gets updated |
|---|---|
| Claude Code | plugin marketplaces and every installed plugin (`claude plugin update`) |
| Codex | Git plugin marketplaces (`codex plugin marketplace upgrade`) |
| Gemini CLI | extensions (`gemini extensions update --all`) |
| All agents | skill folders that are git clones, in `~/.claude/skills`, `~/.codex/skills`, `~/.gemini/skills`, `~/.config/opencode/skills`, `~/.config/opencode/skill`, `~/.cursor/skills`, `~/.copilot/skills`, `~/.codeium/windsurf/skills`, `~/.gemini/antigravity/skills` and `~/.agents/skills` |
| All agents | skills installed with the [`skills`](https://github.com/vercel-labs/skills) CLI (`skills update -g`, run through `pnpm dlx`) |
| All agents | skills installed with `gh skill install` (`gh skill update --all`) |

It never discards your work:

- A git-cloned skill with local changes is skipped, and pulls are fast-forward only.
- A Claude Code plugin whose update would run a marketplace-declared command is skipped,
  so you can review that command by hand.
- Skills you wrote yourself have no upstream and are left alone.
- claude.ai-synced skills (`~/.claude/skills/synced`) are managed by claude.ai.

Updated plugins take effect after you restart the agent.

## Install

Needs Node 18 or later. `pnpm` and `gh` are optional; their steps are skipped without them.

```bash
git clone git@github.com:sxxrfx/skills-update.git
ln -s "$PWD/skills-update/bin/skills-update" ~/.local/bin/skills-update
```

## Usage

```
skills-update                      # update everything now
skills-update auto on              # run daily at 10:00 (macOS launchd)
skills-update auto on --at 08:30   # pick the time
skills-update auto off             # remove the schedule
skills-update auto status          # whether it's on, and the last run's result
skills-update log                  # full log of scheduled runs
```

The schedule is a launch agent at `~/Library/LaunchAgents/local.skills-update.plist`, and
scheduled runs log to `~/Library/Logs/skills-update.log`. On Linux, schedule
`skills-update` with cron instead.

## Adding an agent

Agents are listed in the `AGENTS` table at the top of
[`bin/skills-update`](bin/skills-update): a name, the CLI to look for, its skill folders
relative to your home directory, and an optional `update` function for the agent's own
plugin manager.

## License

MIT
