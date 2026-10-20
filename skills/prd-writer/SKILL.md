---
name: prd-writer
description: Write a PRD (product requirements document) as a Claude Doc, from a Jira ticket, Slack thread, notes or a one-line ask. Use when the user wants a PRD, product spec or requirements doc drafted or refreshed.
---

# PRD writer

Drafts a PRD from whatever the user hands over, grounds it in Jira, Slack and the code, then
asks targeted questions only where the draft is thin. The finished PRD is a Claude Doc that
reviewers comment on.

A PRD records **what** to build and **why**, and how we'll know it worked. The **how** belongs
in a design doc: name constraints and rabbit holes, and leave cache choices, schemas and
endpoints out.

## 1. Read the ask and its sources

Collect everything the user gave you and follow every link:

- **Jira**: `jira issue view <KEY> --plain --comments 20` with
  [`jira-cli`](https://github.com/ankitpokhrel/jira-cli), or an Atlassian connector when that's
  what the session has. Read linked issues the same way when they bear on scope.
- **Slack**: read the whole thread, not only the first message, with the Slack connector.
- **Notes, docs, a sentence in the session**: take them as given.

Note who asked, when and where, with links. Tag each claim as you go:

- **Confirmed**: first-hand knowledge or data backs it.
- **Reported**: the requester said it; nobody has checked.
- **Unknown**: nobody has said.

Keep the requester's proposed solution separate from the problem. It's one input to the
Solution section, not the problem statement.

## 2. Size it

Pick the template tier from [`templates.md`](templates.md):

- **One-pager**: one team, one or two repos, about a sprint or less, nothing risky.
- **Full PRD**: several teams or repos, an external commitment (partner, revenue, deadline), or
  it touches player identity or sign-in, payments or entitlements, or player privacy and data.

When it's borderline, pick the full PRD and confirm the tier in step 6.

## 3. Open the doc

Load the `anthropic-skills:docs` skill and create the doc skeleton now: title, byline, and a
`pending` block per section of the chosen tier. Title it `<JIRA-KEY>: <plain title>` when
there's a ticket (`PROJ-123: Let players link a second sign-in method`),
otherwise the plain title. Tell the user the doc is up, and keep them posted in one short line
per section as you fill it.

## 4. Feasibility pass against the code

A few minutes' look, not a design session. Read local clones of the affected repos when they're
on disk; otherwise use `gh search code --owner <org> "<term>"` or a shallow clone into the
scratchpad. Look for:

- **Existing coverage**: a config field, flag or setting that already does most of what's asked.
- **Risk**: identity, payments, entitlements, privacy.
- **Cross-team reach**: another team's service or the gateway would have to change.
- **The real contract**: when scope involves an existing API, enumerate its actual operations
  from the routes, OpenAPI or SDK surface rather than from the ask.

Cite file paths. Findings feed Constraints, Rabbit holes and Dependencies.

## 5. Draft every section

Fill each pending block with its section from the template. The quality bar:

- **Problem before solution.** Who is affected, what they're trying to do, what goes wrong
  today, the workaround, and the evidence (with its Confirmed / Reported / Unknown tag).
- **Measurable metrics.** Each has a baseline, a target, a timeframe and a measurement method.
  When the baseline is unknown, the metric says so and an open question asks who can get it.
- **Non-goals with reasons.** List what a reader would assume is in scope, each with why it's
  out. Exclusions the user hasn't stated go to step 6 for an explicit yes before they stand.
- **Testable requirements.** Number them `FR-1`, `FR-2`, … with a MoSCoW priority. Acceptance
  criteria in Given / When / Then, with thresholds instead of words like "fast".
- **Constraints apart from requirements.** A compliance rule or a platform limit is a
  constraint; what the system must do is a requirement.
- **Owned open questions.** Each has an owner and is marked blocking or non-blocking.
- **Diagrams where they carry the mechanism.** A user journey, the flow across services, or
  states a purchase moves through: draw it (Claude Docs `topic.diagram`) with labelled arrows.
  A sentence wins when it says it faster.
- **Plain prose.** Lead each section with a short paragraph a stakeholder can read top to
  bottom; save lists and tables for genuinely enumerable things (requirements, metrics, open
  questions). The doc states the current plan: no doc history or "previously we thought".

Where you lack material, write what you have and leave the gap visible as an open question.
Invented numbers are the worst outcome; a visible Unknown is fine.

## 6. Fill the gaps with the user

List the sections still thin after step 5: unknown baselines, unconfirmed exclusions, missing
evidence, an unclear persona, an unsure tier. Ask about them with `AskUserQuestion`, four per
call at most, the weakest first. Draw each option from what you found (the two studios in the
thread, the config field that nearly covers it) and put the option the evidence supports first.
Ask nothing you could look up.

Apply each round's answers to the doc before the next round. Stop when every section meets the
step 5 bar, or the user says to leave the rest as open questions. "I don't know" is an answer:
record it as Unknown with an owner.

## 7. Review pass

Reread the doc against the step 5 bar, section by section. Fix what you can; anything you
can't becomes an open question. Then check the whole: does every requirement trace to the
problem, and does every metric measure the problem rather than the output?

## 8. Humanize

When a humanizer skill is installed (such as [`humanizer`](https://github.com/blader/humanizer)),
load it and run it over every section's prose, table cells included. Skip this step without one. Keep facts, IDs (`FR-n`, tickets), code spans, links and the Confirmed / Reported /
Unknown tags exactly as they stand. Apply the result to the doc: replace a whole block when its
bold or other formatting has to go, since a same-text replace keeps the marks.

## 9. Report

End with the doc link, the tier, the blocking open questions with their owners, and the one
risk most likely to change scope.

## Refreshing an existing PRD

When the user points at an existing PRD doc, read it, change only the sections they name, keep
their wording elsewhere, and rewrite changed sections as the current state with no change
narration.
