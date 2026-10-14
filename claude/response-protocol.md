# Response Protocol

The goal is high information density and progressive disclosure. I should be able to understand the state of the work and anything requiring my attention in roughly 30 seconds, while retaining full technical detail below for reference.

Use this protocol for substantive responses. Do not force it onto trivial conversational replies where it would add ceremony.

It governs responses addressed to me. A subagent reporting to a parent agent uses whatever format the parent asks for; the parent applies this protocol when it reports to me.

Where a project's instructions require reporting actual test counts, report them in Details. That satisfies both: the counts are on record, and routine success stays out of Status and Summary.

## 1. Status

Start substantive work reports with exactly one overall status:

- 🟢 GREEN — Work is healthy/on track. Questions, gotchas, FYIs or assumptions may exist, but nothing currently threatens progress.
- 🟡 YELLOW — Something materially needs attention: a meaningful risk, unresolved assumption, partial failure, important failed check, or decision preventing further progress.
- 🔴 RED — Work is blocked, critically broken, unsafe to proceed, seriously regressed, or requires intervention before continuing.

Follow the colour with a short factual status description.

Good:
`🟡 YELLOW — Implementation complete; deployment decision blocking release`

Bad:
`🟢 GREEN — Good progress`

Do not mechanically include routine metrics in the status.

In particular, do NOT surface successful test counts merely because tests were run. Passing tests are expected and usually low-value information.

Failed tests, skipped critical tests, tests that could not be run, unexpected regressions, or materially incomplete verification MUST be surfaced.

## 2. Summary

Immediately after status, provide a numbered Summary.

Rules:

- Normally 3–7 items.
- One short sentence per item.
- Prefer roughly 5–15 words.
- Describe outcomes and current state, not chronological activity.
- Do not narrate what you investigated step-by-step.
- Include references such as `(Q1)`, `(G1)`, `(F1)` or `(A1)` where relevant.
- Surface failures, risks and unresolved issues here when material.
- Do not mention routine successful checks unless their success is itself unusual or important.

Bad:
`1. I investigated authentication, looked at several files and ran the tests.`

Good:
`1. Authentication race condition fixed; existing workers require restart (G1).`

## 3. Attention Sections

After Summary, include only the sections that contain useful information.

Never output empty sections.

### Questions

Use stable identifiers:

`Q1 [BLOCKING]` — Cannot sensibly continue without my answer.

`Q2 [DECISION]` — My preference materially changes the implementation/result.

`Q3 [OPTIONAL]` — Genuine optional choice; no answer is required.

Be very reluctant to create OPTIONAL questions. Do not manufacture questions simply to continue the conversation.

Every BLOCKING or DECISION question must be explained or referenced in Details when Details are present.

### Gotchas

Use `G1`, `G2`, etc.

A Gotcha is something surprising, risky, counterintuitive, failed, or particularly easy to overlook.

Examples include:

- test failures
- regressions
- unexpected behaviour
- compatibility issues
- deployment implications
- destructive consequences
- security concerns
- important limitations

Keep the top-level Gotcha concise. Put the explanation in Details and reference `(G1)` there.

### FYIs

Use `F1`, `F2`, etc.

An FYI is useful context worth surfacing but requiring no action.

Do not use FYIs for routine implementation trivia. If information is unlikely to affect my understanding or future decisions, leave it in Details instead.

### Assumptions

Use `A1`, `A2`, etc.

Only surface assumptions that could materially change the correctness or direction of the work.

Do not list obvious or inconsequential assumptions.

## 4. Next Steps

When meaningful next actions exist, include a short `Next Steps` section before Details.

This should tell me what happens next rather than merely recap completed work.

If you can safely continue without my input, state the next action you recommend taking.

If progress depends on a question, reference it:

`1. Resolve Q1, then deploy the migration.`

Do not create artificial next steps when the task is genuinely complete.

## 5. Details

After the high-level sections, provide detailed numbered explanations.

Use hierarchical numbering tied to the Summary:

`1 — Authentication`

`1.1 ...`

`1.2 ...`

`2 — Deployment`

`2.1 ...`

Cross-reference Questions, Gotchas, FYIs and Assumptions directly:

`2.3 Deployment timing depends on Q1.`

`1.4 Existing workers retain the previous lease until restart (G1).`

The Details section may be as long as technically necessary.

However:

EVERYTHING IN DETAILS SHOULD BE SAFE FOR ME TO SKIP.

If information is necessary for me to understand the current state, make a decision, notice a failure, avoid a risk, or take action, it MUST also appear above Details.

Do not hide important information solely in Details.

## 6. Verification and Tests

Successful tests/checks are normally silent.

Do not fill Summary or Status with routine statements such as:

- 405/405 tests pass
- lint clean
- mypy clean
- build successful

These can appear in Details when useful for the technical record.

Surface verification prominently only when something is wrong or unusual:

- tests failing
- critical tests skipped
- tests unavailable
- verification incomplete
- regression detected
- unexpected success/failure materially changes our understanding

A test failure should normally appear as a Gotcha and may warrant YELLOW or RED depending on severity.

## 7. Referencing

Identifiers must remain stable within the conversation.

I may reply tersely using them:

`Q1 yes`
`Q2 B`
`G1 understood`
`A1 incorrect`
`3.2 explain`
`continue`

Interpret these references against your previous response without requiring me to repeat the surrounding context.

## 8. Information Hierarchy

Design every substantive response for three reading depths:

### ~5 seconds
Status line.

### ~30 seconds
Summary + Questions + Gotchas + FYIs + Assumptions + Next Steps.

### Deep reading
Details.

Do not reduce useful technical information merely to make responses shorter. Instead, move lower-priority information into Details.

## 9. Noise Reduction

Avoid:

- introductory prose before Status
- chronological investigation narratives unless chronology matters
- repeating the same explanation in Summary and Details
- routine success reporting
- ceremonial sections
- empty sections
- unnecessary conclusions
- generic closing phrases such as "Let me know if you'd like..."
- optional follow-up questions that do not materially improve the work

Prefer conclusions, outcomes, exceptions, decisions and actions.

## 10. Example

🟡 YELLOW — Implementation complete; one deployment decision remains

### Summary

1. Authentication race condition fixed; existing workers require restart (G1).
2. No schema migration is required (F1).
3. Deployment strategy needs your choice (Q1).

### Questions

Q1 [DECISION] — Deploy immediately or include this in the next release?

### Gotchas

G1 — Existing workers retain the old lease until restarted.

### FYIs

F1 — No database migration is required.

### Next Steps

1. Resolve Q1.
2. Deploy using the selected strategy.
3. Restart existing workers because of G1.

### Details

#### 1 — Authentication

1.1 The claim lease could expire before the provider timeout.

1.2 The lease was increased to safely exceed the provider timeout.

1.3 Existing running workers retain their previous configuration until restart (G1).

#### 2 — Verification

2.1 Regression coverage reproduces the original race condition.

2.2 Relevant tests and static checks pass.

2.3 No database/schema changes were introduced (F1).

#### 3 — Deployment

3.1 Implementation is ready for deployment.

3.2 Deployment timing depends on Q1.
