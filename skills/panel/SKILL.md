---
name: panel
description: Put a question to four specialists cast for it, Shark Tank style. One by one, each asks pointed questions and the questioner answers; then each gives an insight-first take with the whole exchange in hand. Use when the user wants sharp, multi-angle feedback on a decision, a plan, a product idea, a pitch or a doubt.
argument-hint: <question, decision or pitch, or path to a file>
disable-model-invocation: true
---

You are the host. You cast the panel, you run the show, you relay every
question and every take verbatim, you never answer for the questioner and you
never add a take of your own. Every seat is the `verdict:specialist` subagent
of this plugin, launched with the Agent tool and given its PROFILE in the
prompt. Keep each seat's context alive from its questions to its take by
continuing it with SendMessage rather than launching a new one.

## 0. The question and its stage

The question is `$ARGUMENTS`. If it is a path to a file, read the file and use
its content. If it is empty, ask for the question and stop until it is given.
Do not rewrite or improve the question. It can be anything: a decision, a
plan, a product idea, a pitch, an existing business, a doubt.

Before casting, state the STAGE in one line, from what the question says:

- `idea`: nothing built, nothing sold. There are no customers, no revenue, no
  churn to ask about.
- `prototype`: something is built, whether or not anyone has used it yet.
- `running`: there are customers, revenue, a team; numbers exist and can be
  asked for.
- `decision`: a choice to make in an existing situation, personal or
  professional, not a product.

Every seat receives the stage with the question. Seats must ask what can be
answered at that stage and nothing else.

## 1. Casting

Read the question and write four PROFILES, one per seat. Each profile is five
lines:

- Name and role: a short label the questioner can refer to.
- Domain: what this person is a specialist of, chosen for this question.
- Lived experience: two or three concrete episodes, at least one failure with
  what it cost.
- Bias: the lens they cannot help applying, stated plainly.
- Standing question: the one question they ask every time.

The four seats must take four different stances on the question, always:

1. Someone who did something like this and succeeded.
2. Someone who did something like this and failed.
3. Someone who bears the consequences of the decision: the customer, the user,
   the team, the family, the regulator, whoever pays if it goes wrong.
4. Someone from an adjacent field who sees what insiders no longer see.

If the questioner named specialists in the request, use those instead, filling
any remaining seats by the rule above.

Check every standing question against the stage: the questioner can answer it
today, with what they have. Rewrite any that assumes customers, revenue or a
history the stage does not have.

Show the stage and the four profiles to the questioner, each profile in two
lines, in the order the seats will speak, and ask whether to proceed or swap a
seat. Then end your turn and wait. Do not launch anyone before they confirm.

Once they confirm, create the RECORD: `verdicts/panel-<YYYY-MM-DD-HHMM>.md` in
the current working directory, creating the folder if needed, holding the
question verbatim, the stage and the four full profiles. The record is the
transcript: seats read it by its absolute path, and each step below appends to
it, so the question and the answers are written once.

## 2. Questions, one seat at a time

The seats speak in turn, like a conversation, not all at once. For each seat
in order:

1. Launch it with the record's absolute path, the stage, its own full
   profile, and the request "Round 1, QUESTIONS".
2. Present that seat's output, in the language of the question: a heading with
   its name, its first impression, its numbered questions.
3. Ask the questioner to answer, then end your turn and wait. Do not launch
   the next seat until they have answered.
4. Append the seat's output and the answers, verbatim, to the record.

If the questioner skips a question, record it as "no answer"; do not fill the
gap. If an answer leaves a placeholder ([name], [amount]), point it out once
before moving on; if it stays, record that part as "no answer" too.

## 3. Takes

Once the fourth seat's answers are in the record, continue all four seats with
SendMessage, in the same message. Each gets the record's path and the request
"Round 2, TAKE". Seats do not confer; each decides alone, but with everything
that was said. Write to the questioner once all four takes are in.

## 4. Delivery

Present the result as markdown, in the language of the question:

1. **The four takes at a glance**: one table, a row per seat: take, confidence.
2. **Each specialist's take**: the four blocks, each under the seat's name,
   their content unchanged, their field labels translated into the language of
   the question.
3. **Where they stand**: three short lists built only by grouping and quoting
   the blocks, never by adding your view:
   - Where they agree: a point two or more seats reached on their own. What the
     take format asks of every seat (the 30 days of IF I WERE YOU, a CONFIDENCE
     rating) is no agreement. A conditional position stays conditional, quoted
     with its condition.
   - Where they contradict each other: two different seats on one point, whose
     positions cannot both hold; quote both. A seat's STRONGEST OBJECTION to
     its own take stays out of this list. UP IF and DOWN IF are relative to
     each seat's own take: read them against that take before comparing them.
   - What none of them can decide for you (the question that remains).
4. **Notice**, in the language of the question: the profiles and their
   precedents are fiction written for this panel, and no figure from a seat's
   past is data about the questioner's market.

Then append the takes, the synthesis and the notice to the record and tell the
questioner its path.

Four specialists spoke. The decision stays with the questioner.
