---
name: "code-check"
description: "Multi-dimension code quality check. Scan codebase health across 5 dimensions, generate fix list, guide repairs. Invoke when you say 'health check', 'code quality', 'tech debt', or 'how healthy is my code'."
status: "stable"
version: "1.0"
---

# Code Check — Codebase Health Check

Assess the overall health of your codebase across multiple dimensions. Find problems before they find you.

## Core Principle

**Scan everything first, then fix. Never fix while scanning.**

This skill has two strict phases:
- **Phase A: Full Scan** — Only check, never fix. Collect ALL issues, generate a fix list.
- **Phase B: Guided Repair** — Fix items one by one, update list after each fix.

## Red Lines (Never Violate)

- Never skip any dimension, even if it seems irrelevant
- Never fix issues during the scan phase
- Never assume the user already knows about issues
- Never mark DONE while critical issues remain unfixed

## Phase A: Full Scan (Check Only, No Fixes)

### Step 1: Scan the Codebase
1. **File count and structure** — How many files? What types?
2. **Dependencies** — Check package.json, requirements.txt, etc.
3. **Test coverage** — Are there tests? How many?
4. **Configuration** — Linting, formatting, CI/CD setup

### Step 2: Health Dimensions (Score 0-100 each)

#### Code Quality (0-100)
- Linting errors
- Type errors
- Code complexity
- Duplicate code

#### Test Health (0-100)
- Test coverage percentage
- Test pass rate
- Test quality (assertions per test)

#### Dependency Health (0-100)
- Outdated dependencies
- Security vulnerabilities
- Unused dependencies

#### Documentation Health (0-100)
- README exists and is current
- API documentation
- Code comments quality

#### Architecture Health (0-100)
- File organization
- Module coupling
- Circular dependencies
- Dead code

### Step 3: Generate Fix List

Output the complete list WITHOUT fixing anything:

```
═══════════════════════════════════════
FIX LIST — Code Check
═══════════════════════════════════════

Overall Score: X/100
Scan Time: YYYY-MM-DD HH:MM

| # | Issue | Dimension | Severity | Location | Status |
|---|-------|-----------|----------|----------|--------|
| 1 | [description] | Code Quality | 🔴 Critical | file:line | Pending |
| 2 | [description] | Code Quality | 🟡 Warning | file:line | Pending |
| 3 | [description] | Dependencies | 🔴 Critical | package.json | Pending |
| 4 | [description] | Test Health | 🟡 Warning | tests/ | Pending |
| 5 | [description] | Architecture | 🟢 Suggestion | src/ | Pending |

### Score Breakdown
| Dimension | Score | Status |
|-----------|-------|--------|
| Code Quality | X/100 | 🟢/🟡/🔴 |
| Test Health | X/100 | 🟢/🟡/🔴 |
| Dependencies | X/100 | 🟢/🟡/🔴 |
| Documentation | X/100 | 🟢/🟡/🔴 |
| Architecture | X/100 | 🟢/🟡/🔴 |

### Quick Wins (High impact, low effort)
[List items that are cheap to fix but have big impact]
═══════════════════════════════════════
```

After showing the list, ask the user:

> Scan complete! Found N issues.
>
> How would you like to proceed?
> - **Fix one by one** — I fix from highest severity down, updating the list after each
> - **Fix critical only** — Only fix 🔴 critical issues, leave 🟡🟢
> - **View only** — I won't fix, you decide what to do
> - **Skip repair** — Record the list, move on

## Phase B: Guided Repair (Fix by List)

For each item to fix:
1. **Identify** — Tell user: `Fixing #N: [issue description]`
2. **Root cause** — If needed, do root cause analysis
3. **Fix** — Implement the fix
4. **Verify** — Confirm the fix works
5. **Update list** — Change status from Pending to Fixed

After each fix, show updated progress:

```
Progress: 3/8 (✅ Fixed 3 / ⬜ Pending 4 / ⏭️ Skipped 1)

| # | Issue | Severity | Status |
|---|-------|----------|--------|
| 1 | [issue1] | 🔴 | ✅ Fixed |
| 2 | [issue2] | 🔴 | ✅ Fixed |
| 3 | [issue3] | 🟡 | ✅ Fixed |
| 4 | [issue4] | 🟡 | ⬜ Next |
| 5 | [issue5] | 🟡 | ⬜ Pending |
| 6 | [issue6] | 🟢 | ⬜ Pending |
| 7 | [issue7] | 🟢 | ⏭️ Skipped |
| 8 | [issue8] | 🟢 | ⬜ Pending |
```

6. **Ask** — After each fix:
   > #N fixed!
   > - Type **"continue"** to fix next item
   > - Type **"skip"** to skip current item
   > - Type **"pause"** to save list and stop
   > - Type **"done"** to end repair and generate final report

## Final Report

```
═══════════════════════════════════════
CODE CHECK — FINAL REPORT
═══════════════════════════════════════

Score Before: X/100
Score After: Y/100
Items Fixed: M/N

### Fix Results
| # | Issue | Severity | Result |
|---|-------|----------|--------|
| 1 | [issue1] | 🔴 | ✅ Fixed |
| 2 | [issue2] | 🟡 | ✅ Fixed |
| 3 | [issue3] | 🟡 | ⏭️ Skipped |

### Remaining Issues
[List all skipped or unfixed items]

### Next Steps
[Suggestions based on remaining issues]
═══════════════════════════════════════
```

## Anti-Rationalization Table

| Excuse | Rebuttal |
|--------|----------|
| "This project is simple, skip full scan" | Simple projects hide problems too — 5 dimensions, no skipping |
| "Let me fix this quick while scanning" | Phase A: NO FIXES. Scan first, fix later |
| "This dimension doesn't apply" | Score all dimensions. If truly N/A, give 90+ and explain why |
| "The user probably knows about these" | Never assume — output the complete list |
| "Fixing is more important than scanning" | No complete scan = no fix priority. Scan enables fix |

## Decision Tree

```
Check depth decision:
├── During development + daily check → Quick scan: 5 dimensions + key metrics
├── Before launch + quality gate → Standard check: 5 dimensions + full metrics + strict thresholds
├── After refactoring + regression → Deep check: 5 dimensions + baseline comparison + trend analysis
└── CI auto + every commit → Lightweight: key metrics + fast output
```

## Behavior Constraints

- Strict two-phase: scan THEN fix
- Every dimension must be scored
- Fix list must be shown to user before any repair
- Critical issues must be addressed before DONE
- Progress must be updated after each fix
