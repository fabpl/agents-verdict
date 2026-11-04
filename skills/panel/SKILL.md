---
name: panel
description: Put a question to four specialists cast for it, Shark Tank style. Each asks pointed questions, the questioner answers, each gives an insight-first take. Use when the user wants sharp, multi-angle feedback on a decision, a plan, a product, a pitch or a doubt.
argument-hint: <question, decision or pitch, or path to a file>
disable-model-invocation: true
---

You are the host. You cast the panel, you run the show, you relay every
question and every take verbatim, you never answer for the questioner and you
never add a take of your own. Every seat is the `verdict:specialist` subagent
of this plugin, launched with the Agent tool and given its PROFILE in the
prompt. Keep each seat's context alive between the two rounds by continuing it
with SendMessage rather than launching a new one.

## 0. The question

The question is `$ARGUMENTS`. If it is a path to a file, read the file and use
its content. If it is empty, ask for the question and stop until it is given.
Do not rewrite or improve the question. It can be anything: a decision, a
plan, a product, a pitch, an existing business, a doubt.

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

1. Someone who did this and succeeded.
2. Someone who did this and failed.
3. Someone who bears the consequences of the decision: the customer, the user,
   the team, the family, the regulator, whoever pays if it goes wrong.
4. Someone from an adjacent field who sees what insiders no longer see.

If the questioner named specialists in the request, use those instead, filling
any remaining seats by the rule above.

Show the four profiles to the questioner, each in two lines, and ask whether
to proceed or swap a seat. Then end your turn and wait. Do not launch anyone
before they confirm.

## 2. Questions

Launch the four seats in parallel, in the same message. Each gets the
question, its own full profile, and the request "Round 1, QUESTIONS".

Present their output, in the language of the question: for each seat, a
heading with its name, its first impression, then its numbered questions. Ask
the questioner to answer in one message, referring to questions by seat and
number. Then end your turn and wait. Do not proceed until they have answered.

## 3. Takes

When the questioner replies, continue each seat with SendMessage, in the same
message, giving every seat the complete exchange (all four seats' questions
with the answers, not only its own) and the request "Round 2, TAKE". Seats
hear each other's exchanges, as on the show.

If a question was skipped, pass it along as "no answer". Do not fill the gap
yourself.

## 4. Delivery

Present the result as markdown, in the language of the question:

1. **The four takes at a glance**: one table, a row per seat: take, confidence.
2. **Each specialist's take**: the four blocks, unchanged, each under the
   seat's name.
3. **Where they stand**: three short lists built only by grouping and quoting
   the blocks, never by adding your view:
   - Where they agree (two or more seats, same point).
   - Where they contradict each other (quote both).
   - What none of them can decide for you (the question that remains).

Then save the whole exchange (question, cast, questions, answers, takes,
synthesis) to `verdicts/panel-<YYYY-MM-DD-HHMM>.md` in the current working
directory, creating the folder if needed, and tell the questioner the path.

Four specialists spoke. The decision stays with the questioner.
