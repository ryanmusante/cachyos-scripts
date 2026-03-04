# audit-prompts

Structured audit prompts for LLM-driven static and runtime analysis of production shell scripts.

## Files

| File | Purpose |
|---|---|
| `fish-audit-prompt.txt` | 254-check audit prompt (238 static + 16 runtime) for Fish shell scripts |
| `CHANGELOG.txt` | Prompt version history extracted from the main prompt |
| `LICENSE` | MIT license |

## What it does

The Fish audit prompt drives a multi-pass analysis of production Fish scripts that manage system configuration, sudo, credentials, embedded configs, and multi-mode dispatch with backup/verification subsystems.

It runs in three passes:

1. **PASS 1 — Gather + Analyze** (phases 1–11, 13–15): parallel data collection with `rg`/`fd`/`bat`, disk-based analysis, 238 static checks covering syntax, routing, error handling, scope, security, embedded configs, cross-refs, architecture, patterns, documentation, testing, gap analysis, and delta review.
2. **PASS 2 — Runtime** (phase 12): 16 runtime checks exercising `--help`, `--version`, exit codes, `NO_COLOR`, `--dry-run`, `--lint`, and mode-specific behavior.
3. **PASS 3 — Finalize**: severity collation, summary table, fixes spec with exact `str_replace`-ready before/after pairs, and packaged deliverables.

## Usage

Paste or attach `fish-audit-prompt.txt` as a system/user prompt, then upload the Fish script to audit. The prompt is self-contained — it includes setup, tool fallbacks, data collection commands, the full checklist, and output formatting.

### Requirements

The prompt expects a Linux environment with:

- `fish` (for syntax validation and function extraction)
- `rg` (ripgrep — `grep -P` fallback provided)
- `fd` (`find` fallback provided)
- `bat` (`cat`/`sed` fallback provided)

### Deliverables

Each audit produces:

- `audit-<script>-<version>.txt` — full findings with severity tags
- `fixes-spec-<version>.txt` — actionable fix instructions with unique before/after text for `str_replace`

## Versioning

The prompt version (`Version:` header) tracks prompt evolution. The `Derived from:` field tracks which script version the prompt was last validated against. See `CHANGELOG.txt` for version history.
