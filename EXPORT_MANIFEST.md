# EXPORT MANIFEST — what may leave this machine

This project mixes **two kinds of files**: the public deliverable, and a local
development harness (project memory banks, agent config, scratch tooling).
Only the allowlist below may be published, pushed, zipped for distribution, or
uploaded anywhere. Everything in the denylist is local-only.

> Enforcement lives in **`.gitignore`** (git) and **`.npmignore`** (npm, which
> otherwise falls back to `.gitignore`). This file is the human-readable
> source of truth for *why* those exclusions exist.

---

## ALLOWLIST — public distribution surface

| Path | What it is |
|---|---|
| `index.html` | **Deliverable 1** — PeopleCore Enterprise HRIS Sandbox & Academy (single file, 773,891 B) |
| `landing.html` | Marketing landing page (83,909 B) |
| `assets/screenshots/*.png` | Screenshot suite referenced by `landing.html` (20 files) |
| `.gitignore` | The exclusion rules themselves (must be tracked to work) |
| `.npmignore` | The npm-side mirror of those rules |
| `EXPORT_MANIFEST.md` | This document |

---

## DENYLIST — never distribute

| Path | Why it is internal |
|---|---|
| `PROJECT_MEMORY.md` | Persistent knowledge bank, sole-owned by `memory-keeper`, local-only by policy |
| `PROJECT_LOGS.md` | Chronological session history; local-only by policy |
| `AGENTS.md` | Root agent instructions — contains the `opencode.json` integrity hash and the deny-rule fallback mechanics |
| `.agents/` | Canonical session-hook definitions (`Hook 1` / `Hook 2`) |
| `.opencode/` | Agent definitions (`memory-keeper.md`) |
| `opencode.json` | Harness config — 10 permission `deny` rules governing the memory banks |
| `scratch/` | Development scratch: validators, capture harnesses, splice tooling, diff/scan output |
| `.env`, `.env.*`, `*.env`, `*.local` | Environment / local override files (none present today; matched defensively) |
| `.vscode/`, `.idea/` | Editor configuration |
| `*.log`, `*.bak`, `*.tmp`, `node_modules/`, `dist/`, `build/`, `coverage/`, `.cache/` | Tooling and build residue |

---

## Pre-flight check before any public push

1. **Confirm the repo is not already dirty with internals:**
   ```
   git status --porcelain
   ```
2. **Confirm the denylist is actually ignored:**
   ```
   git check-ignore -v PROJECT_MEMORY.md PROJECT_LOGS.md AGENTS.md opencode.json scratch/ .agents/ .opencode/
   ```
   Every path above must print a matching rule.
3. **Confirm nothing internal is already staged or tracked:**
   ```
   git ls-files | findstr /I "PROJECT_ AGENTS opencode scratch .agents .opencode .env"
   ```
   This must print **nothing**.
4. **For a zip/tarball distribution**, ship only the allowlist explicitly, e.g.:
   ```
   git archive -o release.zip HEAD index.html landing.html assets/
   ```

---

## Notes

- The workspace is **not yet a git repository**; `.gitignore` / `.npmignore`
  are staged protection for when it is initialised. No `git init` has been run.
- There is **no `package.json`**, so the project is not published to npm;
  `.npmignore` is defensive.
- No secrets, API keys, tokens or credentials exist in the repo — the sensitive
  material here is *internal working state* (memory banks, agent config,
  scratch output), not keys.
