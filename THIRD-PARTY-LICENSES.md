# Third-Party Licenses

This repository vendors, under `third_party/`, unmodified copies of a small
number of skills, commands, and agent definitions from two upstream MIT-licensed
projects. They are included so the orchestrate skill's sub-skill registry works
out of the box; installing the upstream plugins is recommended for the latest
versions.

## Superpowers

- **Upstream:** https://github.com/obra/superpowers
- **Author:** Jesse Vincent
- **License:** MIT — full text in `third_party/superpowers/LICENSE`
- **Vendored version:** 6.2.0
- **Vendored portions (unmodified):**
  - `skills/writing-plans/`
  - `skills/test-driven-development/`
  - `skills/using-git-worktrees/`

## ECC (Everything Claude Code)

- **Upstream:** https://github.com/affaan-m/ECC
- **Author:** Affaan Mustafa
- **License:** MIT — full text in `third_party/ecc/LICENSE`
- **Vendored version:** 2.0.0-rc.1
- **Vendored portions (unmodified):**
  - `skills/santa-method/`
  - `commands/code-review.md`
  - `agents/security-reviewer.md`

Everything outside `third_party/` is original work licensed under this
repository's `LICENSE` (MIT, Copyright (c) 2026 Giusse Macri).
