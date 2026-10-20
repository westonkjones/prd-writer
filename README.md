# prd-writer

A [Claude Code](https://claude.com/claude-code) plugin that drafts a PRD (product requirements document) from whatever you hand it: a Jira ticket, a Slack thread, meeting notes or a one-line ask. The finished PRD is a Claude Doc that reviewers can comment on.

A PRD here records what to build, why, and how you'll know it worked. Implementation choices are left to a design doc.

## How it works

```
sources ──► size ──► open doc ──► feasibility ──► draft ──► questions ──► review ──► link
(Jira,      (one-     (skeleton    (read the       (every     (only where   (against
 Slack,      pager or  with a       code: existing  section)   the draft is  the bar)
 notes)      full)     section      coverage,                  thin)
                       per tier)    risk, reach)
```

- **Sources**: follows every link, reads whole Slack threads, and tags each claim Confirmed, Reported or Unknown. The requester's proposed solution is kept apart from the problem.
- **Size**: a one-pager for single-team work of about a sprint; a full PRD for cross-team work, external commitments, or anything touching identity, payments, entitlements or privacy.
- **Feasibility**: a short look at the code for existing coverage, risk, cross-team reach and the real API contract, with file paths cited.
- **Draft**: measurable metrics with baselines, non-goals with reasons, numbered `FR-n` requirements with MoSCoW priority and Given / When / Then acceptance criteria, owned open questions, and diagrams where they carry the mechanism. Gaps stay visible as open questions; numbers are never invented.
- **Questions**: asks about the weakest sections, up to four at a time, with options drawn from what it found.

Point it at an existing PRD doc to refresh the sections you name and leave the rest alone.

## Install

```
claude plugin marketplace add westonkjones/wes-skills
claude plugin install prd-writer@wes-skills
```

It's listed in the [wes-skills](https://github.com/westonkjones/wes-skills) marketplace.

## Use

```
/prd-writer:prd-writer PROJ-123                          # from a Jira ticket
/prd-writer:prd-writer https://example.slack.com/archives/C0/p0   # from a Slack thread
/prd-writer:prd-writer let studios schedule store sales  # from a one-line ask
```

Or ask in plain words, like "write a PRD for this ticket".

## Requirements

- A Claude Code session with the Claude Docs connector, where the PRD is written.
- Optionally, Jira access ([`jira-cli`](https://github.com/ankitpokhrel/jira-cli) or an Atlassian connector) and the Slack connector, to read sources.
- Optionally, `gh`, for searching code in repos you don't have cloned.
- Optionally, a humanizer skill such as [`humanizer`](https://github.com/blader/humanizer) for a final prose pass.
