# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Warp, etc.) working in this repository.

## What this repo is

A **Claude Code / OpenCode skill** implemented entirely as Markdown. The runtime artifact is `SKILL.md`: the agent reads its YAML frontmatter (metadata) followed by the editor prompt. There is no build step for the skill itself; the only code in this repo is the validation script under `scripts/`.

This skill is intentionally separate from [humanizer](https://github.com/blader/humanizer). Do not merge them, and do not add cross-repo dependencies. If a change belongs in humanizer's general AI-pattern detection instead of this skill's house style, it goes in that repo, not this one.

## Key files

- `SKILL.md` — the skill itself. YAML frontmatter (`name`, `description`, `license`, `metadata.version`) followed by the rule set: sentence rules, word rules, structure rules, tone rules, 13 numbered patterns with before/after examples, a "what not to flag" guardrail section, and voice calibration. **This is the source of truth.**
- `README.md` — for humans: installation, usage, the output contract, the pattern tables, a full worked example, and a version history.
- `.claude-plugin/plugin.json` — Claude Code plugin manifest. Points `skills` at `./` (the repo root, where `SKILL.md` lives).
- `.claude-plugin/marketplace.json` — makes the repo installable via `/plugin marketplace add johnakande/copy-humanizer`.
- `scripts/validate-package.py` — checks that `SKILL.md`, `README.md`, and `plugin.json` haven't drifted apart. No external dependencies.
- `.github/workflows/validate.yml` — runs the validator, a skill-discovery check, and a Claude plugin check on every push and pull request.

## The maintenance contract

`SKILL.md`, `README.md`, and the two `.claude-plugin/*.json` files must stay in sync. When you change behavior or content:

- **Patterns:** the skill currently defines **13 numbered patterns** under "Patterns to catch," headed `### 1.` through `### 13.`. If you add, remove, or renumber any, update the README pattern tables, the "N patterns" references, and `scripts/validate-package.py`'s expected range in the same change.
- **Banned words:** the word list in the "Word rules" section is a single flat list, not numbered. If you add or remove entries, keep the list alphabetized-by-convenience (grouped loosely by theme is fine) and don't duplicate an entry that's already covered by a different phrasing.
- **Version:** version lives in `SKILL.md`'s `metadata.version` frontmatter field, `.claude-plugin/plugin.json`'s `version` field, and README's first "Version History" entry. Bump all three together. Do not add a top-level `version:` field to `SKILL.md` (it belongs under `metadata:`), and do not add `compatibility:` or `allowed-tools:` fields. The validator rejects both.
- **Output contract:** the "deliver final text only, no draft, no summary" rule is the defining difference from humanizer's default (which shows a draft/audit/final loop on request). Don't soften this without the user's explicit sign-off, since it was a deliberate, repeated instruction, not a default this repo drifted into.
- **Non-obvious fixes:** if you change the prompt to handle a tricky failure mode (a rule that kept getting missed, an edge case in the cut list), add a short note to the README version history explaining what was fixed and why.

## Checks

Before publishing a change, run:

```bash
python3 scripts/validate-package.py
npx --yes skills@latest add . --list
```

The GitHub Actions workflow runs the same validator plus `claude plugin validate .` on every push and pull request. A pull request that fails these checks should not be merged.

## Writing style

Use this repo's own house style, from `SKILL.md`, in code comments, documentation, validator error messages, and progress reports: lead with the main point, use active voice and common words, keep sentences and paragraphs short, cut hedging and filler, and use "must" for real requirements. Keep exact identifiers, commands, paths, and the word-for-word text of rules and examples unchanged when the meaning depends on the exact wording.

## Editing SKILL.md

- Preserve valid YAML frontmatter (formatting and indentation).
- The prompt below the frontmatter is the product. Edit it like a careful instruction document, not code.
- Keep the before/after examples grounded in GTM, marketing, and business-writing contexts. Don't pull examples from humanizer's Wikipedia-article-style sources; this skill's audience is different.
- Keep `SKILL.md` at 500 lines or fewer. The validator enforces this.
