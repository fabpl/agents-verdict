# verdict

Pitch an idea, get it judged. A Claude Code plugin that runs entirely on your
Claude subscription: no API key, every role is a Claude Code subagent with its
own isolated context.

Two modes:

| Skill | What happens |
|---|---|
| `/trial <pitch>` | Counsel **for** and counsel **against** each plead, then rebut. Four profiled jurors deliberate separately and return a structured verdict. A clerk tallies the votes and the recurring objections. |
| `/panel <pitch>` | Four partners (growth, finance, product, field) each ask you two or three questions, like *Qui veut être mon associé* / *Shark Tank*. You answer, then each one decides: in or out, offer, conditions. |

The pitch can be inline text or a path to a file. Agents reply in the language
of the pitch.

## Install

Try it without installing:

```bash
claude --plugin-dir /path/to/agents-verdict
```

Install from this repository as a marketplace, inside Claude Code:

```
/plugin marketplace add fabpl/agents-verdict
/plugin install verdict@fabpl
```

## Layout

```
.claude-plugin/plugin.json   plugin manifest
skills/trial/SKILL.md        /trial orchestration
skills/panel/SKILL.md        /panel orchestration
agents/counsel-for.md        pleads for the idea
agents/counsel-against.md    pleads against the idea
agents/juror-*.md            the four jurors (trial mode)
agents/clerk.md              tallies the verdicts (trial mode)
agents/partner-*.md          the four partners (panel mode)
```

## Tuning the cast

Every role is one markdown file in `agents/`. Edit the prompt to change a
juror's biases, swap a partner's specialty, or add a fifth seat (then list it in
the matching `SKILL.md`). The `model:` line in each file's frontmatter picks the
model for that role; jurors and partners default to `sonnet` to spare your
quota, counsel and clerk inherit the session model.

## Design notes

- Each role runs in its own subagent context. Jurors never see each other's
  deliberation, only the pitch and the pleadings, which is what keeps them from
  converging on the first opinion voiced.
- Verdicts use a fixed block (`VERDICT`, `SCORE`, `REASONS`, `KILLER_RISK`,
  `WOULD_CHANGE_MIND_IF`) so the clerk can tally them.
- Every prompt carries an explicit anti-flattery rule: judge as if your own
  money were at stake, always name one thing that could kill the project.
- All roles share one underlying model, so disagreement comes from the prompts,
  not from genuinely different minds. Keep the profiles sharp.
