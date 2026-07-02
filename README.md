# `/dataviz` skill (Claude Code 2.1.198)

These are the files that make up the `/dataviz` skill, extracted  from the 2.1.198 version of claude-code
Layout mirrors the on-disk skill (`SKILL.md` + `references/*.md` + `scripts/*`).

| File | Role |
|---|---|
| `SKILL.md` | Skill entry point / 7-step procedure |
| `references/choosing-a-form.md` | Which chart type, or is it even a chart |
| `references/color-formula.md` | The color method (four jobs, six checks) |
| `references/palette.md` | The reference palette instance |
| `references/marks-and-anatomy.md` | Mark specs |
| `references/components.md` | Stat tile / KPI component specs |
| `references/interaction.md` | Interaction + accessibility rules |
| `references/anti-patterns.md` | What to avoid |
| `scripts/validate_palette.js` | Runnable palette validator (Node CLI + embeddable module) |
| `scripts/validate_palette.py` | Same validator, Python |


One informational note: `validate_palette.js` is dual-mode and, when
embedded as `<script type="module">` in a generated chart artifact, auto-runs from
`document.body.dataset.palette`. Its only effect is `console.table` / `console.warn`
diagnostics: no DOM write, no `eval`, no `innerHTML`, so it is not a sink even if
`data-palette` were attacker-controlled.

Kept here as reference material only.
