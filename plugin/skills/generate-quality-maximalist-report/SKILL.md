---
name: generate-quality-maximalist-report
description: >
  Generates a quality-first report for a quality-maximalist manager — someone who asks about test
  coverage, edge cases, and completeness before approving work, and who hesitates to ship without
  evidence that the unhappy paths were considered. Use this skill after running
  useless-report:classify-manager-style and getting a quality_maximalist profile, or directly when
  your manager always asks "did we consider all the edge cases?" or "what's our test coverage?".
  Also triggers when the user needs to justify readiness for release, document what was tested,
  or communicate known gaps with acceptable rationale.
---

# generate-quality-maximalist-report

## Goal

The quality-maximalist manager's confidence comes from evidence of thoroughness — not from being told "it's ready". Show them what was tested, what was found, what gaps remain and why they're acceptable, and what monitoring is in place. Their concern is rarely irrational: they've seen things break after too-confident ships.

The report should answer: **how do we know this is safe to ship?**

## Reference

Read `../../archetypes/quality-maximalist.md` for the full communication strategy, signals, and what to avoid.

## Input (accept any combination)

- Summary of work done and what was shipped or prepared for shipping
- Test coverage information (unit, integration, E2E)
- Edge cases that were considered and handled
- Known gaps and rationale for accepting them
- Monitoring or observability in place
- Outstanding risks

## Output structure

```
## Quality & Readiness Report — [Period]

### 1. Summary
[2-3 sentences: what was built, where it stands]

### 2. Quality coverage
[What was tested: unit tests, integration tests, manual testing, staging validation]
- Unit tests: [coverage or description]
- Integration tests: [what was covered]
- Manual / exploratory: [what was verified]

### 3. Edge cases addressed
[Explicit list of edge cases that were considered and handled]
- [Edge case 1]: handled by [mechanism]
- [Edge case 2]: handled by [mechanism]

### 4. Known gaps
[Honest list of what was not tested or covered, with rationale for why it's acceptable to ship]
- [Gap 1]: not covered because [reason]. Acceptable because [argument].

### 5. Test status
[Overall test suite status: passing / failing / partially passing — with context]

### 6. Monitoring in place
[What observability exists to catch issues post-ship: alerts, dashboards, error tracking]

### 7. Open risks
[Any residual risks after all mitigations — rated low/medium/high with rationale]

### 8. Recommendation
[Ship / Hold / Ship with conditions — with one sentence of reasoning]
```

## Rules

- Never present "everything is fine" without evidence. State what was tested and what was found.
- Known gaps must always have a rationale. "We didn't test X because [reason] and we accept this because [argument]" is acceptable. A gap with no explanation is not.
- If there's nothing in a section (e.g., no known gaps), say "None identified" — don't omit the section.
- The Recommendation section matters — give a clear signal. This manager needs to feel they have enough information to approve.
- Be honest about partial coverage. Overstating completeness will damage trust when issues appear.

## The controlled verbosity rule

> Verbosity serves the reader's need for certainty, not padding.

The edge cases and known gaps sections should be as complete as the work warrants. This manager reads them. The summary section should be brief.
