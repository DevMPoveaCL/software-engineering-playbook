# 🤝 GitHub Collaboration

> **Why this matters:** Programming is a team sport. GitHub is where the pros play.

---

## 🧠 The Mental Model: The Restaurant Review System

Imagine GitHub is **Yelp for code**. Instead of reviewing restaurants:

- **Issues** = Bug reports and feature requests (1-star vs 5-star reviews)
- **Pull Requests (PRs)** = You're asking the owner (maintainer) to add YOUR recipe to their menu
- **Reviews** = Other chefs taste-testing before approval
- **Merge** = The dish is officially on the menu

### The Flow

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  You (Contributor)           GitHub              Maintainer       │
│         │                     │                     │             │
│         │  fork repo          │                     │             │
│         │ ─────────────────▶  │                     │             │
│         │                     │                     │             │
│         │  clone to local     │                     │             │
│         │ ◀───────────────────│                     │             │
│         │                     │                     │             │
│         │  make changes       │                     │             │
│         │ ◀───────────────────│                     │             │
│         │                     │                     │             │
│         │  commit + push      │                     │             │
│         │ ─────────────────▶  │                     │             │
│         │                     │                     │             │
│         │  open PR            │                     │             │
│         │ ─────────────────▶  │                     │             │
│         │                     │     review + request│             │
│         │                     │ ◀──────────────────│             │
│         │                     │     changes        │             │
│         │  address feedback   │                     │             │
│         │ ─────────────────▶  │                     │             │
│         │                     │     approve + merge│             │
│         │                     │ ◀──────────────────│             │
│         │                     │                     │             │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🐛 Issues: The Bug Report System

### When to Open an Issue

| Good Reasons | Bad Reasons |
|-------------|------------|
| Found a bug with steps to reproduce | "This doesn't work" (no details) |
| Feature request with clear requirements | "Add cool feature X" (vague) |
| Security vulnerability discovered | Questions answerable by docs |

### Writing a Good Issue (The Template)

```markdown
## 🐛 Bug Description
Clear description of what went wrong.

## 📋 Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. See error

## ✅ Expected Behavior
What should happen instead.

## 🔍 Environment
- OS: Windows 11
- Browser: Chrome 120
- Node: 20.x

## 📸 Screenshots
[If applicable]
```

---

## 🔀 Branches: Parallel Universes

### ASCII: Branch Topology

```
                           feature/user-logout
                          /
                         /   ← You work here
                        /
        ─────────────●───────────────────────────────▶ (time)
                    /
                   /
              merge point
                   /
                  /
            ──────●──────────────────────▶ main
                 /
                /
        start of project
```

### Branch Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/<description>` | `feature/user-logout` |
| Bug Fix | `fix/<description>` | `fix/payment-null-pointer` |
| Hotfix | `hotfix/<description>` | `hotfix/security-patch` |
| Chore | `chore/<description>` | `chore/update-dependencies` |

---

## 📥 Pull Requests: The Core Workflow

### What is a Pull Request?

A PR is a **formal request to merge your code into another branch**. It signals:
1. "I'm done with my changes"
2. "I want someone to review"
3. "Please integrate my work"

### Creating a PR (The Checklist)

```bash
# 1. Make sure your branch is clean and up-to-date
git checkout feature/my-feature
git pull origin main

# 2. Run tests locally (DO THIS BEFORE PUSHING)
npm test  # or whatever your test command is

# 3. Push your branch
git push -u origin feature/my-feature

# 4. Open PR on GitHub (or use CLI)
gh pr create --title "feat: add user logout flow" --body @PR_TEMPLATE.md
```

---

## ⛓️ Chained Pull Requests: Protecting Reviewer Focus

> **This is critical for cognitive load protection.**

### The Problem: Mega-PRs

```
❌ MEGA-PR #847: "Refactor everything and add 50 features"
Changed files: 847
Lines changed: 12,847
Review time needed: 4-6 hours
Reviewers: overwhelmed, rushed, missed critical bugs
```

### The Solution: Chained PRs

```
✅ CHAINED PRs — Small, focused, safe

PR #1: 🔧 Infrastructure
       "Setup TypeScript + clean config"
       Changes: 200 lines
       Review time: 10 min ✅

PR #2: 🏗️ Core Domain
       "Extract User entity and validation"
       Changes: 300 lines
       Review time: 15 min ✅
       (Depends on: #1)

PR #3: ✨ Feature
       "Add user logout flow"
       Changes: 150 lines
       Review time: 10 min ✅
       (Depends on: #2)

PR #4: 🎨 Polish
       "Update styles and animations"
       Changes: 100 lines
       Review time: 5 min ✅
       (Depends on: #3)
```

### ASCII: Chained PR Dependency Chain

```
┌─────────────────────────────────────────────────────────────────┐
│  PR #1: Infrastructure (Foundation)                            │
│  ┌──────────────────┐                                            │
│  │ TS Config +     │  ← Reviewer approves, merges               │
│  │ Base Types      │                                            │
│  └──────────────────┘                                            │
│           │                                                       │
│           │ (PR #2 depends on #1)                                │
│           ▼                                                       │
│  ┌──────────────────┐                                            │
│  │ User Entity +    │  ← Reviewer sees clean base, approves     │
│  │ Validation Rules │                                            │
│  └──────────────────┘                                            │
│           │                                                       │
│           │ (PR #3 depends on #2)                                │
│           ▼                                                       │
│  ┌──────────────────┐                                            │
│  │ Logout Flow      │  ← Small change, easy to reason about      │
│  │ Implementation  │                                            │
│  └──────────────────┘                                            │
│           │                                                       │
│           │ (PR #4 depends on #3)                                │
│           ▼                                                       │
│  ┌──────────────────┐                                            │
│  │ UI Polish        │  ← Incremental improvement, no surprises    │
│  │ + Animations     │                                            │
│  └──────────────────┘                                            │
└─────────────────────────────────────────────────────────────────┘

BENEFIT: Each reviewer can focus 10-15 min. No context overflow.
         If PR #2 breaks, you know EXACTLY where to look.
```

### Why Chained PRs Protect Everyone

| Without Chains | With Chains |
|----------------|-------------|
| 3,000 line PR = 2 hour review | 200 line PR = 10 min review |
| Fatigue = missed bugs | Fresh eyes = better feedback |
| "What was this PR about again?" | Clear purpose per PR |
| Reverting = manual archaeology | Revert = delete branch |
| Conflict resolution nightmare | Small merges = easy merges |

---

## 👀 Code Review: The Core Exchange

### Reviewer Checklist

```markdown
## Review Checklist
- [ ] Code solves the stated problem
- [ ] No hidden side effects
- [ ] Tests cover happy + edge cases
- [ ] Naming is clear (self-documenting)
- [ ] No secrets committed
- [ ] Follows project conventions
```

### Comment Conventions

| Symbol | Meaning |
|--------|---------|
| `nit:` | Minor nitpick (optional fix) |
| `suggestion:` | Improvement idea |
| `question:` | Need clarification |
| `blocker:` | Must fix before merge |

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Open Issue BEFORE coding** | Avoids building wrong thing |
| **Use PR templates** | Ensures all needed info is provided |
| **Keep PRs under 400 lines** | Easier to review, less cognitive load |
| **Link PR to Issue** | `Closes #123` auto-closes issue on merge |
| **Respond to ALL comments** | Communication is collaboration |
| **Use `draft` PRs for WIP** | Signals "not ready for review" |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't merge without review** | You will miss bugs and style issues |
| **Don't ignore reviewer feedback** | They caught something real |
| **Don't resolve threads yourself** | Let the author resolve — they need to learn |
| **Don't review your own PR** | defeats the purpose of collaboration |
| **Don't push to main directly** | Bypasses quality control |

---

## 🔧 GitHub CLI (Workplace Productivity)

```bash
# Sync your fork
gh repo sync your-org/your-fork

# View all PRs
gh pr list --state=open --reviewer=@me

# Checkout PR locally
gh pr checkout 123

# Merge locally
gh pr merge 123 --squash --delete-branch

# Review a PR
gh pr review 123 --approve --comment "LGTM!"
```

---

[⬅️ Back to Parent](../README.md)
