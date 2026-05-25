# NEVER

Institutional memory. Injected at session start by minions. Each entry is a lesson learned.

Format: `NEVER [action] BECAUSE [reason]`

New entries are added by `@chronicler`. Never delete entries — add a correction instead.

---

## Template

```
NEVER [specific action] BECAUSE [concrete reason in this project's context]
Context: [file/module/pattern where this applies, or "global"]
Added: [ISO 8601 date]
```

## Example entries

```
NEVER use Array<T> syntax BECAUSE our ESLint config enforces T[] and Array<T> causes lint failures on every commit
Context: global (all TypeScript files)
Added: 2026-01-01

NEVER catch exceptions without re-throwing or logging BECAUSE silent failures make debugging take 10x longer
Context: global
Added: 2026-01-01
```

---

*Add project-specific rules below this line using @chronicler.*
