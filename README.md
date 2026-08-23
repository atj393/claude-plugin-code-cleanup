<div align="center">

# code-cleanup

**A Claude Code plugin that runs a careful, low-risk codebase cleanup pass.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-informational)](.claude-plugin/plugin.json)
[![Marketplace](https://img.shields.io/badge/marketplace-atj393%2Fclaude--plugins-6E4AFF)](https://github.com/atj393/claude-plugins)
[![Agents](https://img.shields.io/badge/agents-9-blue)](#the-seven-tracks)

</div>

---

A **lead agent** coordinates **seven specialist workers**, one per track, and a **final
reviewer**. Workers assess first and only apply edits that are clearly high-confidence and
low-risk. Medium-risk findings are reported back, not implemented.

The point of the split is blast radius. Each worker is scoped to one class of change, so a
mistake in the dead-code pass cannot quietly ride along inside a type consolidation, and the
reviewer audits one diff rather than eight unrelated intentions.

## Project status

- **Source:** open source under **MIT**, version `1.0.0`.
- **Distribution:** the [atj393/claude-plugins](https://github.com/atj393/claude-plugins) marketplace, pinned by commit SHA.
- **Contents:** 9 agent definitions and 9 skill playbooks. No build step and no dependencies.
- **Requirements:** Claude Code (CLI, desktop app, or a supported IDE).

## Install

From any Claude Code session:

```
/plugin marketplace add atj393/claude-plugins
/plugin install code-cleanup
```

Then confirm:

```
/plugin
/help
```

You should see `code-cleanup` listed under `/plugin` and `/code-cleanup:run` in `/help`. Plugins
install at **user scope**, so they are available in every project on your machine.

## Usage

**Whole repo**

```
/code-cleanup:run
```

**Specific paths**

```
/code-cleanup:run src/auth src/api
```

**With an extra instruction**

```
/code-cleanup:run src "prioritize zero behavior change and focus on types + dead code first"
```

## The seven tracks

| # | Track | What it looks for |
|---|---|---|
| 1 | **dedup** | Repeated logic, copy-pasted functions, redundant abstractions |
| 2 | **types** | Duplicated shared types that have drifted across files |
| 3 | **dead-code** | Unused exports, orphaned files, unreferenced functions, manually verified |
| 4 | **cycles** | Circular imports that hurt maintainability or testability |
| 5 | **strong-types** | `any` and weak `unknown` placeholders that have a clear correct type |
| 6 | **errors** | Silent `catch` blocks, misleading defaults, swallowed failures |
| 7 | **slop** | Deprecated paths, placeholder stubs, AI-generated comments that narrate edits |

After all tracks, a **reviewer** audits the diff as a skeptical senior engineer.

Each worker runs on Sonnet with a 30-turn cap. The lead gets 60 turns, the reviewer 25. The caps
are there so a track that finds nothing stops quickly rather than looking for work.

## Operating rules

The lead and every specialist follow these rules:

- Be conservative. Preserve behavior.
- Keep diffs small and reviewable.
- Prefer local fixes over architectural rewrites.
- Only implement high-confidence, low-risk changes.
- Medium-risk items are reported, not implemented.
- Low-confidence items are left untouched.
- Never claim success without naming the checks actually run.

Parallel assessment is encouraged. Parallel editing only happens when file ownership is clearly
separated.

## Known limitations

- The plugin edits your working tree. Run it on a clean branch so the diff is reviewable.
- Verification is only as good as the checks your repository already provides. If there is no
  test or typecheck command, workers cannot prove behavior was preserved.
- The marketplace pins this plugin by commit SHA, so changes here reach users only after the
  marketplace entry is bumped.

## Uninstall

```
/plugin uninstall code-cleanup
/plugin marketplace remove atj393-plugins
```

The marketplace is removed by its name, `atj393-plugins`, not by the repository name.

## Local development

If you are working on the plugin itself, clone it and register a local marketplace that points
at your working copy:

```
git clone https://github.com/atj393/claude-plugin-code-cleanup.git
```

Create a `marketplace.json` in a sibling folder pointing at the clone, then:

```
/plugin marketplace add <path-to-local-marketplace>
/plugin install code-cleanup
```

Edits to the clone take effect on the next `/plugin update`.

## Layout

```
code-cleanup/
├── .claude-plugin/plugin.json
├── README.md
├── LICENSE
├── agents/
│   ├── code-cleanup-lead.md
│   ├── code-cleanup-dedup.md
│   ├── code-cleanup-types.md
│   ├── code-cleanup-dead-code.md
│   ├── code-cleanup-cycles.md
│   ├── code-cleanup-strong-types.md
│   ├── code-cleanup-errors.md
│   ├── code-cleanup-slop.md
│   └── code-cleanup-review.md
└── skills/
    ├── run/SKILL.md                  entry point, /code-cleanup:run
    ├── dedup-playbook/SKILL.md
    ├── types-playbook/SKILL.md
    ├── dead-code-playbook/SKILL.md
    ├── cycles-playbook/SKILL.md
    ├── strong-types-playbook/SKILL.md
    ├── errors-playbook/SKILL.md
    ├── slop-playbook/SKILL.md
    └── review-playbook/SKILL.md
```

## License

[MIT](LICENSE).
