---
name: "bug-hunter"
description: "Systematic debugging with root cause analysis. Find the real cause before fixing. Invoke when you see bugs, errors, stack traces, 'why is this broken', or unexpected behavior."
status: "stable"
version: "1.0"
---

# Bug Hunter — Root Cause Debugging

Iron Law: **No fixes without root cause.** If you don't know why it broke, you don't know what to fix.

## Core Principle

**Find the real cause, then fix it. Never fix symptoms.**

This skill has two strict phases:
- **Phase A: Root Cause Investigation** — Only investigate, never fix. Collect all hypotheses.
- **Phase B: Guided Fix** — Fix root causes one by one, verify each fix.

## Red Lines (Never Violate)

- Never guess a fix without reproducing the bug
- Never swallow exceptions with try-catch without investigating
- Never fix symptoms instead of root cause (e.g., adding null check without finding why null appears)
- Never skip regression testing — every fix must have a regression test

## Common Failure Modes

| Failure Mode | Symptom | Strategy |
|-------------|---------|----------|
| **Cannot reproduce** | User reports bug but can't reproduce locally | Add diagnostic logging → Ask user to reproduce → Collect logs → Re-analyze |
| **Wrong root cause** | Fixed an assumption but problem persists | Verify original bug disappears after each fix. If not, rule out that hypothesis |
| **Fix introduces new bug** | Fixed A but broke B | Run regression tests + check related code after every fix |
| **Investigation loop** | Reading same file repeatedly with no progress | 3 reads of same file with no new insight → STOP → Change angle or ask user |
| **Over-investigation** | Tracing into framework internals | Mark as external dependency issue, find workaround |

## Phase A: Root Cause Investigation (Investigate Only, No Fixes)

### Phase 1: Reproduce and Observe
1. **Reproduce the issue.** Get the exact error, the exact input, the exact state.
2. **Read the error message.** Actually read it. Most bugs announce themselves.
3. **Check recent changes.** `git log --oneline -10` and `git diff HEAD~1` — what changed?
4. **Check the environment.** Different OS? Different Node version? Different config?
5. **Ask the user:** "What were you doing when this broke? What did you expect to happen?"

### Phase 2: Trace the Path
1. **Follow the stack trace.** Go to each file, read the code around each frame.
2. **Check the data.** What are the actual values? Add logging if needed.
3. **Check the flow.** Is the code path what you expect? Trace the execution.
4. **Check dependencies.** Did a dependency update break something?
5. **Check configuration.** Env vars, config files, feature flags.

### Phase 3: Form Root Cause Hypotheses
1. **State the root cause hypothesis.** "I believe the bug is in [file]:[line] because [reason]."
2. **List alternative hypotheses.** What else could cause this?
3. **Design a test for each hypothesis.** What would prove or disprove each one?
4. **Rank by likelihood.** Most likely first.

**Gate:** Do NOT proceed to Phase 4 until you have a specific, testable root cause hypothesis.

### Generate Investigation List

```
═══════════════════════════════════════
INVESTIGATION LIST — Bug Hunter
═══════════════════════════════════════

Problem: [user-reported issue]
Time: YYYY-MM-DD HH:MM

| # | Root Cause Hypothesis | Likelihood | Location | Impact | Status |
|---|----------------------|------------|----------|--------|--------|
| 1 | [hypothesis 1] | 🔴 Very High | file:line | [impact] | Pending |
| 2 | [hypothesis 2] | 🟡 Medium | file:line | [impact] | Pending |
| 3 | [hypothesis 3] | 🟢 Low | file:line | [impact] | Pending |

### Confirmed Root Cause
[If confirmed, state it. If not, mark as "Pending verification"]
═══════════════════════════════════════
```

## Phase B: Guided Fix (Fix by List)

### Phase 4: Fix and Verify
For each root cause to fix:
1. **Identify** — Tell user: `Fixing #N: [root cause description]`
2. **Verify hypothesis** — Confirm this root cause is actually causing the problem
3. **Fix the root cause.** Not the symptom. The ROOT CAUSE.
4. **Write a test that would have caught this bug.** Regression test.
5. **Verify the fix.** Run the test. Reproduce the original issue — it should be gone.
6. **Check for similar issues.** Search for the same pattern elsewhere.
7. **Commit with clear message.** "fix: [what was wrong] caused by [root cause]"
8. **Update list** — Change status from Pending to Fixed

## Final Report

```
═══════════════════════════════════════
BUG HUNTER — FINAL REPORT
═══════════════════════════════════════

Original Problem: [user-reported issue]
Confirmed Root Cause: [final confirmed root cause]

### Fix Results
| # | Hypothesis | Likelihood | Result |
|---|-----------|------------|--------|
| 1 | [hypothesis 1] | 🔴 | ✅ Fixed (confirmed root cause) |
| 2 | [hypothesis 2] | 🟡 | ⏭️ Ruled out (not root cause) |
| 3 | [hypothesis 3] | 🟢 | Not verified |

### Regression Tests Added
[List of regression tests added]

### Remaining Risks
[Other potentially related issues]
═══════════════════════════════════════
```

## Anti-Patterns to Avoid

- **Shotgun debugging** — Changing things randomly to see what works
- **Fixing symptoms** — Adding a null check without understanding why null appears
- **Assuming causation** — "It worked before" is not a root cause
- **Skipping reproduction** — If you can't reproduce it, you can't verify the fix

## Decision Tree

```
Investigation depth decision:
├── P0 + affects entire system → Deepest: full investigation + multi-angle analysis
├── P1 + affects core feature → Standard: reproduce → log analysis → locate → fix
├── P2 + affects non-core → Lightweight: quick locate → fix → verify
└── P3 + UX issue → Lightest: confirm issue → simple fix
```

## Completion Status

- **DONE** — Root cause identified, fix implemented, test added, verified
- **DONE_WITH_CONCERNS** — Fix applied but root cause uncertain or similar issues exist
- **BLOCKED** — Cannot reproduce or cannot determine root cause
- **NEEDS_CONTEXT** — Need more information from the user
