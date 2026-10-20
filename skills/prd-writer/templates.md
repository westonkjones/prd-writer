# PRD templates

Section lists for the two tiers. Drop an optional section only when it has nothing to say,
never to make the PRD look more settled than it is.

## One-pager

1. **Problem**: who, what goes wrong today, the workaround, evidence.
2. **Solution**: the shape of the answer at the level a reviewer can react to, not a design.
3. **Success metrics**: baseline → target, timeframe, how measured.
4. **Scope and non-goals**: what's in; what's out and why.
5. **Requirements and acceptance criteria**: `FR-n` with MoSCoW, Given / When / Then.
6. **Rabbit holes and dependencies**: what could swallow time, other teams involved.
7. **Open questions**: owner, blocking or not.
8. **References**: Jira, Slack, related docs and PRs.

## Full PRD

1. **Summary**: two to four sentences: what this is and the outcome it drives.
2. **Background**: current state, prior work, why now.
3. **Problem and evidence**: who's affected, impact, workaround; claims tagged Confirmed /
   Reported / Unknown.
4. **Goals and success metrics**: baseline → target, timeframe, how measured; guardrail
   metrics that must not regress.
5. **Non-goals**: each with its reason.
6. **Personas and use cases**: who, and the concrete scenarios to support.
7. **User journeys**: step-by-step flows; draw the main one.
8. **Requirements**: functional `FR-n` with MoSCoW priority; non-functional (performance,
   security, privacy, accessibility, localisation) with thresholds.
9. **Acceptance criteria**: Given / When / Then per `FR-n`.
10. **Constraints and assumptions**: compliance, platform limits, what's taken as given.
11. **Risks, rabbit holes and dependencies**: likelihood, mitigation; other teams' work by team
    and repo.
12. **Rollout and timeline**: milestones, flag or staged rollout, target dates if known.
13. **Open questions**: owner, blocking or not.
14. **References**: Jira, Slack, designs, related docs and PRs.
