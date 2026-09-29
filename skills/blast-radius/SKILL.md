---
name: blast-radius
description: |
  USE before editing code to measure what the change can break and pick how careful to be. Standalone: git + grep only, no hooks, no MCP. Rates high/medium/low from evidence, then waits for dev confirmation.
  Triggers (es+en): "blast radius", "what could this break", "how risky is this", "before I touch", "qué rompo", "qué tan riesgoso".
  Manual invoke: Claude `/blast-radius` or Codex `$blast-radius`.
license: MIT
---

# blast-radius

Rate risk from evidence, not gut feel. No edits until dev confirms.

## Approach
Trust the prompt. Files named → use them. Else `git diff --name-only`. No files either way → ask. No list, no rating.

## Signals (compute only what's missing)
- **External consumers**: `grep -rn` each file's exports **and** its route/command name. Shell script parsing output counts as consumer. Count only those outside the file's module.
- **Copies**: identical file elsewhere? `find . -name <file>` then `diff`. Copy = every change must land twice.
- **Sensitive area**: auth, payments, DB schema/migrations, API contracts, deploy/CI config, secrets handling.
- **Shape vs behavior**: does output, contract, or observable effect change? Cite `file:line` that proves it.

## Level · first matching rule wins
| If | Level |
|---|---|
| sensitive area, >1 external consumer, copies, or multiple modules | 🔴 high |
| behavior changes, one module | 🟡 medium |
| shape only (rename, format) | 🟢 low |

- Signal not computable → `not computed: <why>`, take highest level among signals that were.
- Level can go up, never down.
- Don't invent consumers or dismiss them by name.

## Output (≤8 lines), then stop and wait for "go"
```
level: 🔴 high
rule: copies — utils/Mongo.js is identical in 3 other modules
files: [utils/Mongo.js]
consumers: 0 external (grep "Mongo." outside worker/)
shape/behavior: shape — only swaps logger (index.js:34)
validate: <existing repo command: test, build, lint>
```

## No
- No edits before dev confirms.
- No new tests or tooling unless asked; point to what repo already runs.
- No long report: rating + evidence only.
