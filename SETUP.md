# Setup

Minions are agent definition files (MINION.md) designed to run inside AI coding tools that support custom agents or slash commands. Each tool has its own deployment path.

---

## Claude Code

Claude Code supports custom slash commands via `.claude/commands/`.

```bash
# In your project root
mkdir -p .claude/commands

# Copy each minion as a slash command
for dir in blueprint sentinel chronicler bloodhound ledger gatekeeper; do
  cp /path/to/minions/$dir/MINION.md .claude/commands/$dir.md
done
```

Invoke with `/blueprint`, `/sentinel`, etc.

---

## OpenCode

OpenCode supports custom skills via `~/.config/opencode/skills/`.

```bash
for dir in blueprint sentinel chronicler bloodhound ledger gatekeeper; do
  mkdir -p ~/.config/opencode/skills/$dir
  cp /path/to/minions/$dir/MINION.md ~/.config/opencode/skills/$dir/SKILL.md
done
```

---

## Cursor / Windsurf / Cline

These tools support custom instructions via `.cursor/rules/` or equivalent. Copy each MINION.md to your rules directory and invoke them by name in your prompt.

---

## Any tool with agent files

1. Find the directory where your tool loads agent/skill/command definitions
2. Copy each `<name>/MINION.md` to that directory (rename if needed)
3. Set up the artifact directory:

```bash
mkdir -p .minions/artifacts
touch .minions/STATE.md
touch NEVER.md
```

---

## Project state files

Minions expect these files at the project root or in `.minions/`:

| File | Purpose | Created by |
|------|---------|-----------|
| `NEVER.md` | Institutional memory: project-specific never/always rules | `@chronicler` |
| `.minions/STATE.md` | Cross-session state: current phase, decisions, gate records | `@gatekeeper`, `@blueprint` |
| `.minions/BUDGET.md` | Token budget status | `@ledger` |
| `.minions/artifacts/` | Per-task artifacts passed between minions | various |

Add `.minions/artifacts/` to `.gitignore` if you don't want task artifacts committed. Keep `NEVER.md` and `.minions/STATE.md` in version control.

---

## .gitignore recommendation

```gitignore
# Minion artifacts (task-specific, not worth committing)
.minions/artifacts/
.minions/BUDGET.md

# Keep these in git
# NEVER.md
# .minions/STATE.md
```
