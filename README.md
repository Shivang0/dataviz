# `/dataviz` skill (Claude Code 2.1.198) — extracted source + security verdict

These are the files that make up the `/dataviz` skill, extracted faithfully from the
2.1.198 SEA binary (`bin/claude.exe`) where they ship as embedded JS template literals.
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

## Security verdict: CLEAN (no High/Critical)

We hunted this surface for (a) a code-execution sink in the "runnable validator" and
(b) untrusted-data-into-HTML/SVG injection in generated chart artifacts. Both are negative:

- **Validators are numeric-only color math.** Input is one CLI arg: a comma-separated
  list of hex strings, parsed with `parseInt(...,16)`. No `eval`, `exec`, `child_process`,
  `require`, file read, or network fetch anywhere in either script. A malformed "hex"
  yields `NaN`, not execution.
- **No unsafe interpolation.** The skill contains zero vulnerable HTML/SVG string-building
  examples, and `references/interaction.md` explicitly mandates `textContent` /
  `createTextNode` for untrusted labels (CSV headers, tool output, API responses),
  naming the threat model directly.

One informational note (not a vulnerability): `validate_palette.js` is dual-mode and, when
embedded as `<script type="module">` in a generated chart artifact, auto-runs from
`document.body.dataset.palette`. Its only effect is `console.table` / `console.warn`
diagnostics: no DOM write, no `eval`, no `innerHTML`, so it is not a sink even if
`data-palette` were attacker-controlled.

Kept here as reference material only.
