# CLAUDE.md — Project Instructions for Claude Code

This file is automatically read by Claude Code at the start of every session.
Follow these rules for all work on this project.

---

## Version Control — MANDATORY

**Every code change to `vessel-multi-zone V3.html` MUST include a version bump and a CHANGELOG entry. No exceptions.**

### Version numbering (`MAJOR.MINOR.PATCH`)

| Digit | When to increment | Examples |
|:------|:------------------|:---------|
| PATCH (Z) | Bug fix, style tweak, label change, no new controls | iframe fix, color correction |
| MINOR (Y) | New feature, new panel/tab, new mode, new component | unit system, logo, spec tab |
| MAJOR (X) | Architecture rework, breaking state change, file rename | rewrite of build system |

### Two places to update — always both, never just one

1. **`vessel-multi-zone V3.html`** — two locations:
   - `<title>` tag: `<title>Vessel Thermal Mapping Dashboard vX.Y.Z</title>`
   - Script constant: `const APP_VERSION="X.Y.Z";`

2. **`CHANGELOG.md`** — prepend a new entry at the top (below the `---` separator after the header):
   ```markdown
   ## [vX.Y.Z] — YYYY-MM-DD
   ### Added / Changed / Fixed
   - Description of what changed and why
   ```

### Commit message must reference the version
Include the version in the git commit subject or body, e.g.:
```
feat: add thermal export button (v3.5.0)
```

---

## File Map

| File | Purpose |
|:-----|:--------|
| `vessel-multi-zone V3.html` | Main single-file app — React 18 + Three.js r128 + Babel |
| `Vessel thermal mapping configurator.html` | Launcher wrapper — fetches latest app from GitHub, shows version badge |
| `CHANGELOG.md` | Full version history — update with every change |
| `CLAUDE.md` | This file — project instructions for Claude Code |
| `README.md` | Public-facing overview |
| `TECHNICAL_SPEC.md` | Math, coordinate systems, architecture docs |
| `changes.md` | Proposal spec and original feature tracker |

---

## Code Style

- **No build step** — single `.html` file, all CDN. Never introduce a build tool.
- **React without JSX** — use `e(tag, props, children)` pattern (`const e = React.createElement`). No JSX syntax anywhere.
- **Inline styles only** — no external CSS files, no CSS-in-JS libraries.
- **Internal state in SI** — all lengths stored in metres, temperatures in °C. Unit conversion at display layer only via `makeUnits()`.
- **Targeted edits** — file is large (~1000+ lines). Prefer `Edit` over full rewrites. Read relevant sections before editing.
- **No speculative features** — only implement what the user explicitly asked for.

---

## GitHub

- Repo: `jammu-ai/sru-vessel-thermal` (public)
- Branch: `master`
- After every change: `git add`, `git commit` (with version reference), `git push`
- Launcher fetches raw file from: `https://raw.githubusercontent.com/jammu-ai/sru-vessel-thermal/master/vessel-multi-zone%20V3.html`

---

## Checklist before every commit

- [ ] `APP_VERSION` updated in `vessel-multi-zone V3.html`
- [ ] `<title>` tag updated in `vessel-multi-zone V3.html`
- [ ] New entry added at top of `CHANGELOG.md`
- [ ] Commit message references the new version
- [ ] `git push` completed
