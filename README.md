# 🟡 minions

> Evidence-based AI agent minions for production-grade coding workflows, built on the [10 Claude Code Principles](https://jdforsythe.github.io/10-principles/overview/).

Most AI agent advice is built on vibes. Minions are built on science.

Each minion is a tightly-scoped specialist: a brief identity (< 50 tokens), a clear mission, and hard behavioral rules derived from peer-reviewed research. No flattery. No bloat. Just focused agents that do their job reliably.

---

## The Philosophy

Minions follow four core ideas drawn from the 10 Principles:

- **Specialize, don't generalize.** A reviewer defined as "senior SRE" in 30 tokens outperforms one with three paragraphs of backstory (PRISM research). Each minion has one job.
- **Context is precious.** Every token competes for the model's attention. Minion prompts are postcards, not novels.
- **Hard gates, not rubber stamps.** Minions either approve with evidence or block with reasons. "LGTM" with no rationale is a failure mode (MAST FM-3.1).
- **Codify mistakes forever.** When a minion gets something wrong, the lesson goes into its `never/always` list — not just your memory.

---

## Minions

| Minion | Trigger | Mission | Type |
|--------|---------|---------|------|
| [blueprint](minions/blueprint/MINION.md) | `@blueprint [goal]` | Turn a vague goal into a versioned plan artifact before any code is written | solo |
| [sentinel](minions/sentinel/MINION.md) | `@sentinel` | Security & correctness review: finds hardcoded secrets, injection risks, race conditions | specialist |
| [chronicler](minions/chronicler/MINION.md) | `@chronicler [lesson]` | Captures session corrections into NEVER.md so mistakes don't repeat | solo |
| [bloodhound](minions/bloodhound/MINION.md) | `@bloodhound [query]` | Searches the codebase for context before a question is asked of the user | subagent |
| [ledger](minions/ledger/MINION.md) | `@ledger` | Token budget tracker: reports context burn rate and flags runaway pipelines | observer |
| [gatekeeper](minions/gatekeeper/MINION.md) | `@gatekeeper` | Human-gate checkpoint: surfaces the 2-3 highest-stakes decisions before irreversible actions | solo |

---

## Workflows

**Standard development loop:**

`@blueprint [goal]` → implement → `@sentinel` → `@gatekeeper` → commit

**When something goes wrong:**

`@chronicler [what happened]` → update NEVER.md → restart with clean context

**Before a big refactor:**

`@bloodhound [area]` → `@blueprint [refactor plan]` → `@gatekeeper` → implement

**Context hygiene between major steps:**

`@blueprint` → /clear → implement — fresh context for implementation

implement → /clear → `@sentinel` — fresh context for review

---

## Minion Anatomy

Every minion lives in `minions/<name>/MINION.md` with a YAML frontmatter header:

```yaml
---
name: sentinel
description: Security and correctness reviewer. Finds hardcoded secrets, injection risks, and race conditions. Blocks on findings; never rubber-stamps.
model: claude-sonnet-4-5
max_tokens: 4096
---
```

Followed by: **Mission** (one sentence), **Behavioral Rules** (the always/never list with reasons), **Step-by-step protocol**, and **Output format**.

---

## Artifacts

Minions pass context to each other via `.minions/artifacts/`:

| Artifact | Written by | Read by |
|----------|-----------|---------|
| `<slug>-blueprint.md` | blueprint | sentinel, gatekeeper |
| `<slug>-sentinel-report.md` | sentinel | gatekeeper |
| `NEVER.md` | chronicler | all minions (injected at session start) |
| `BUDGET.md` | ledger | all minions |

---

## Setup

```bash
git clone https://github.com/MarianaRL/minions
# Copy minions/ to your AI tool's agent/slash-command directory
# See SETUP.md for tool-specific instructions
```

---

## Adding a Minion

1. Create `minions/<name>/MINION.md` with YAML frontmatter
2. Write the mission, behavioral rules, protocol, and output format
3. Add it to the table above

The only rule: **one mission per minion**. If you're writing "and also", split it.

---

## The Science

- **PRISM persona research** — brief identities outperform elaborate backstories; flattery degrades output
- **Liu et al. (2024) "Lost in the Middle"** — critical context belongs at the edges, not buried in the middle
- **MetaGPT (Hong et al., 2023)** — structured artifacts reduce errors ~40% vs. free dialogue
- **MAST failure taxonomy** — FM-3.1 (rubber-stamp approval) is the most common quality failure in multi-agent systems
- **DeepMind 2025 scaling data** — 7+ agents often performs worse than 4; start small, escalate on evidence
- **CHI 2023 "Why Johnny Can't Prompt"** — combined positive instruction + negative constraint is the strongest prompting pattern

Full breakdown: [10 Claude Code Principles](https://jdforsythe.github.io/10-principles/overview/)

---

## Project State

`.minions/STATE.md` persists cross-session context: current focus area, open decisions, and active `never` rules. Written by `@chronicler`; read by all minions on session start.

---

*Minions: small, focused, relentless.*
