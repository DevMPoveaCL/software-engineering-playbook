# 🐱 Git Fundamentals

> **Why this matters:** Git is a time machine for your code. Master this, and you'll never fear breaking things again.

---

## 🧠 The Mental Model: Photo Album for Code

Imagine you're an architect with a magical camera. Every time you shout "SAVE!", the camera takes a snapshot of your entire project — every file, every line. You can:

- **Travel back in time** to any snapshot
- **Create parallel universes** (branches) to experiment safely
- **Compare snapshots** to see exactly what changed
- **Restore any previous state** instantly

That's Git in a nutshell.

---

## 🏃 Basic Workflow (The Daily Ritual)

```bash
# 1. START: Get the project (first time only)
git clone https://github.com/your-org/your-repo.git

# 2. DAILY CYCLE: Make changes → Save → Upload
git add .                    # Stage ALL changes (the camera focuses)
git commit -m "fix: resolve VAT calculation in cart"  # Take snapshot with label
git push                     # Upload to the cloud (backup your timeline)

# 3. BRANCHING: Create parallel work
git checkout -b feature/discount-banner  # Create new universe
# ... hack hack hack ...
git commit -m "feat: add discount banner component"
git push -u origin feature/discount-banner
```

### ASCII Diagram: The Basic Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      YOUR LOCAL MACHINE                     │
│                                                              │
│   ┌──────────┐    git add .    ┌──────────┐    git commit   │
│   │ Working  │ ──────────────▶ │  Staging │ ──────────────▶│
│   │  Area    │                 │   Area   │                 │
│   └──────────┘                └──────────┘                │
│                                     │                       │
│                              git commit -m "msg"            │
│                                     ▼                       │
│                              ┌──────────┐                  │
│                              │  Local   │ ◄── Your timeline  │
│                              │  Repo    │    lives here     │
│                              └──────────┘                  │
│                                     │                       │
│                                git push                    │
└──────────────────────────────────── │ ──────────────────────┘
                                      ▼
                        ┌────────────────────────┐
                        │      GITHUB CLOUD       │
                        │    (Remote Repository)  │
                        │                         │
                        │   origin/main ──────────┼─── Team sees this
                        └────────────────────────┘
```

---

## 📋 Essential Commands Reference

| Command | What it Does | When to Use |
|---------|--------------|-------------|
| `git init` | Create a new repository | When starting a project from scratch |
| `git clone <url>` | Download a repository | First time getting a project |
| `git status` | Show what's changed | **Always before committing** |
| `git add <file>` | Stage specific file | When you want to commit only one file |
| `git add .` | Stage ALL changes | Most common — stages everything |
| `git commit -m "msg"` | Save snapshot | Every logical unit of work |
| `git push` | Upload to cloud | After every commit (backup!) |
| `git pull` | Download + merge | Before starting new work |

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Commit early, commit often** | Small commits = easy to debug, easy to revert |
| **Write meaningful commit messages** | `fix: correct VAT in checkout` tells you exactly what changed |
| **Check `git status` before committing** | Ensures you're committing what you intend |
| **Use `-m` flag for clear messages** | Inline messages force you to be concise |

### Example: A Good Commit Sequence

```bash
git add calculate_vat.js
git commit -m "fix: correct VAT calculation for EU countries"
# Result: Clean history that's easy to search and revert
```

---

## 🚫 What NOT To Do

| Don't Do This | Why Not | The Alternative |
|---------------|---------|-----------------|
| **Don't commit like a save button** | `git commit -am "fixed some things"` is noise | One logical change per commit |
| **Don't commit secrets/passwords** | They're in history FOREVER | Use environment variables |
| **Don't commit large binaries** | Bloats the repo, slows everything | Use Git LFS or external storage |
| **Don't amend public commits** | Rewrites shared history = chaos for teammates | Just add a new commit |
| **Don't `git add .` when unsure** | May stage files you don't need | Stage intentionally |

### ⚠️ The Password Disaster Scenario

```bash
# WRONG: You accidentally committed credentials
echo "DB_PASSWORD=super-secret123" >> config.env
git add .
git commit -m "update config"
git push  # TOO LATE - it's in the cloud history FOREVER

# RIGHT: Use .gitignore and environment variables
echo "config.env" >> .gitignore
git add .gitignore
git commit -m "chore: add config.env to gitignore"
```

---

## 🎯 The Golden Rules (Workplace Context)

### Rule 1: One Commit = One Logical Unit

Think of commits like **ingredients**, not **dishes**:

- ❌ `git commit -am "updated stuff and fixed bugs"` — What stuff? Which bugs?
- ✅ `fix: resolve null pointer in user fetch` — Clear, searchable, reversible

### Rule 2: Never Rewrite Public History

```bash
# YOUR branch only (safe):
git reset --soft HEAD~1  # Undo last commit, keep changes staged

# SHARED branch (DANGEROUS):
git push --force  # ⚠️ WILL break your teammates' local repos
```

### Rule 3: Branch Naming = Communication

```bash
# Bad names (useless):
git checkout -b bugfix
git checkout -b latest
git checkout -b mybranch

# Good names (self-documenting):
git checkout -b feature/user-logout
git checkout -b fix/payment-race-condition
git checkout -b chore/update-react-19
```

---

## 🔍 Inspection Commands (Debug Your History)

| Command | What it Shows |
|---------|---------------|
| `git log --oneline` | Compact history: one line per commit |
| `git log --graph` | Visual branch diagram |
| `git diff` | What's changed but NOT staged |
| `git diff --staged` | What's staged and ready to commit |
| `git show <commit>` | Full details of a specific commit |
| `git blame <file>` | Who changed each line (blame!) |
| `git grep "pattern"` | Search in tracked files |

---

## 🧪 Stash: The "Parking Lot" for In-Progress Work

When you need to switch context but aren't ready to commit:

```bash
git stash                 # Put current changes in a parking lot
git stash pop            # Retrieve them when ready
git stash list           # See all your stashes
```

### ASCII: Stash Workflow

```
┌─────────────────────────────────────────────────────┐
│  WORKING ON: feature/payment-flow                   │
│  Task: Add discount calculation                     │
│  Status: HALF-DONE (not ready to commit)           │
│                                                      │
│  git stash ──────────────────────────────────┐     │
│       │                                          │     │
│       ▼                                          │     │
│  ┌─────────┐                                     │     │
│  │  STASH  │ ◄── "Discount calc (WIP)"         │     │
│  └─────────┘                                     │     │
│       │                                          │     │
│  git checkout main                                │     │
│  git stash pop  ────────────────────────────────┘     │
│       │                                                  │
│       ▼                                                  │
│  BACK TO: feature/payment-flow                       │
│  Changes restored, ready to continue                 │
└─────────────────────────────────────────────────────┘
```

---

## 📚 Technical Glossary

| Term | Definition |
|------|------------|
| **Repository (Repo)** | The container for your project's entire history |
| **Commit** | A snapshot with a message describing what changed |
| **Branch** | A parallel timeline for isolated work |
| **Merge** | Combining branch changes back into main |
| **Clone** | Downloading a remote repo to your machine |
| **HEAD** | Your current position in the commit timeline |
| **Origin** | The default name for the remote repository |

[⬅️ Back to Parent](../README.md)
