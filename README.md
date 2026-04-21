# code-cleanup

A Claude Code plugin that runs a careful, low-risk codebase cleanup pass.

A **lead agent** coordinates **seven specialist workers** (one per track) and a **final reviewer**. Workers assess first and only apply edits that are clearly high-confidence and low-risk. Medium-risk findings are reported back, not implemented.

---

## Install

From any Claude Code session (CLI, desktop app, or supported IDE):

```
/plugin marketplace add atj393/claude-plugins
/plugin install code-cleanup
```

Then confirm:

```
/plugin
/help
```

You should see `code-cleanup` listed under `/plugin` and `/code-cleanup:run` in `/help`. Plugins install at **user scope** — available in every project on your machine.

---

## Usage

### Whole repo
```
/code-cleanup:run
```

### Specific paths
```
/code-cleanup:run src/auth src/api
```

### With an extra instruction
```
/code-cleanup:run src "prioritize zero behavior change and focus on types + dead code first"
```

---

## The seven tracks

| # | Track | What it looks for |
|---|---|---|
| 1 | **dedup** | Repeated logic, copy-pasted functions, redundant abstractions |
| 2 | **types** | Duplicated shared types that have drifted across files |
| 3 | **dead-code** | Unused exports, orphaned files, unreferenced functions (manually verified) |
| 4 | **cycles** | Circular imports that hurt maintainability or testability |
| 5 | **strong-types** | `any` / weak `unknown` placeholders that have a clear correct type |
| 6 | **errors** | Silent `catch` blocks, misleading defaults, swallowed failures |
| 7 | **slop** | Deprecated paths, placeholder stubs, AI-generated comments that narrate edits |

After all tracks, a **reviewer** audits the diff as a skeptical senior engineer.

---

## Operating rules

The lead and every specialist follow these rules:

- Be conservative. Preserve behavior.
- Keep diffs small and reviewable.
- Prefer local fixes over architectural rewrites.
- Only implement high-confidence, low-risk changes.
- Medium-risk items are reported, not implemented.
- Low-confidence items are left untouched.
- Never claim success without naming the checks actually run.

Parallel assessment is encouraged; parallel editing only happens when file ownership is clearly separated.

---

## Uninstall

```
/plugin uninstall code-cleanup
/plugin marketplace remove atj393-plugins
```

---

## Local development

If you're hacking on the plugin itself, clone it and register a local marketplace that points at your working copy:

```
git clone https://github.com/atj393/claude-plugin-code-cleanup.git
```

Create a `marketplace.json` in a sibling folder pointing at the clone, then:

```
/plugin marketplace add <path-to-local-marketplace>
/plugin install code-cleanup
```

Edits to the clone take effect on the next `/plugin update`.

---

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
    ├── run/SKILL.md                  (entry point — /code-cleanup:run)
    ├── dedup-playbook/SKILL.md
    ├── types-playbook/SKILL.md
    ├── dead-code-playbook/SKILL.md
    ├── cycles-playbook/SKILL.md
    ├── strong-types-playbook/SKILL.md
    ├── errors-playbook/SKILL.md
    ├── slop-playbook/SKILL.md
    └── review-playbook/SKILL.md
```

---

## License

MIT
