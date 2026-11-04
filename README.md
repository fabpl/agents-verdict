# verdict

Pitch an idea, get it judged. A Claude Code plugin that runs entirely on your
Claude subscription: no API key, every role is a Claude Code subagent with its
own isolated context.

Two modes:

| Skill | What happens |
|---|---|
| `/trial <pitch>` | Counsel **for** and counsel **against** each plead, then rebut. Four profiled jurors deliberate separately and return a structured verdict. A clerk tallies the votes and the recurring objections. |
| `/panel <question>` | Four specialists **cast for your question** each ask you two or three pointed questions, like *Qui veut être mon associé* / *Shark Tank*. You answer, then each one gives an insight-first take: position, what you have not seen, strongest objection, precedent, first step. The host groups where they agree, where they contradict, and what none of them can decide for you. |

`/trial` wants an idea to judge. `/panel` takes anything: a decision, a plan,
a product, a pitch, an existing business, a doubt. Input can be inline text or
a path to a file. Agents reply in the language of the input.

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
agents/specialist.md         one seat of the panel, profiled per question (panel mode)
```

## Tuning the cast

Every trial role is one markdown file in `agents/`. Edit the prompt to change
a juror's biases or add a fifth seat (then list it in `skills/trial/SKILL.md`).

The panel has no fixed cast. The host writes four profiles for each question
and shows them before starting; swap a seat then, or name your own specialists
in the command. What stays fixed is the spread of stances: one who did it and
succeeded, one who did it and failed, one who bears the consequences, one from
an adjacent field. That rule lives in `skills/panel/SKILL.md`; the seat's
behaviour and take format live in `agents/specialist.md`.

The `model:` line in each agent's frontmatter picks the model for that role;
jurors and the specialist default to `sonnet` to spare your quota, counsel and
clerk inherit the session model.

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
