# 🐙 Git and Version Control

If you're programming without Git, you're programming on the edge of the abyss. **Git is the programmer's time machine.**

It's a Version Control system that saves the "photo album" of every change you make to your code. If you make a mistake, break the system, or accidentally delete important files, Git lets you travel to the past (literally to a previous version) in seconds and restore everything to normal.

---

## 📊 Objective Table: Git Analysis

| Aspect | Didactic Explanation |
|--------|---------------------|
| **What is it for?** | Track the history of source code changes, coordinate work between multiple programmers, and enable safe experimentation. |
| **What are the benefits?** | Acts as a safety net. Facilitates teamwork (multiple people working on the same file without stepping on each other). |
| **When to use it?** | **ALWAYS**. From minute one when you create a project, even if you're coding alone. It's the number 1 industry standard. |
| **When NOT to use it?** | Don't use Git to store heavy videos, real databases, or security credentials (NEVER upload passwords to history!). |

---

## 🧠 Engineering Best Practices

1. **One Commit = One Logical Unit of Work:**
   A "Commit" (saving a version) should not be used like a "Save Game" button before lunch. It must represent something meaningful.
   - *Bad Commit:* `Fixed some things, changed colors, and updated database`.
   - *Good Commit:* `fix: correct VAT calculation error in shopping cart`.
2. **Branches - Parallel Universes:**
   The main project lives on the `main` branch (the real universe). When you want to experiment or create a new button, create a branch called `feature/new-button`. Modify it without fear. If your experiment goes wrong, you delete it and `main`'s code was never altered. If it works, you merge it.
3. **Don't alter public history:**
   Git lets you rewrite past history (delete old commits). This is fine if the code lives only on your computer. But if the code has been uploaded and shared with your team, **never rewrite history**, or you'll cause massive chaos on everyone else's machines.

> **Didactic Tip:** Git **is not the same as GitHub or GitLab**. Git is the invisible "engine" program on your computer. GitHub is the "social network" in the cloud (Microsoft's virtual hard drive) where we upload our Git-managed code for remote storage.

---

## 📚 Technical Glossary

- **Commit:** A "snapshot" or state capture that permanently saves changes made to the code.
- **Branch:** A parallel alternative timeline to the main code, useful for experimenting in isolation without breaking anything.
- **Merge:** The action of taking changes from a secondary branch and mixing them back into the main project (`main`).
- **Pull Request (PR):** When working in a team, instead of merging code directly, you make a "Request." You ask another developer to review your code and approve it before officially integrating it.

---

## 📂 Learning Path

| Module | Description |
|--------|-------------|
| [🐱 Git Fundamentals](./01-Git-Fundamentals/README.md) | Core commands, staging, commits, branching basics |
| [🤝 GitHub Collaboration](./02-GitHub-Collaboration/README.md) | Issues, PRs, reviews, chained PRs, cognitive load protection |
| [🔄 CI/CD Workflows](./03-CI-CD-Workflows/README.md) | Continuous Integration, Continuous Deployment, automation pipelines |

> **Start here if you're new:** Begin with [Git Fundamentals](./01-Git-Fundamentals/README.md), then move to [GitHub Collaboration](./02-GitHub-Collaboration/README.md) when working with a team.

---
### 🔗 Global Navigation
[⬅️ Previous Topic: Ionic + Angular](../IONIC+ANGULAR+FIREBASE+CAPACITOR/README.md) | [🏠 Master Index](../README.md) | [➡️ Next Topic: Docker](../DOCKER/README.md)
<br>
**[⬇️ Dive In: 01-Git-Fundamentals](./01-Git-Fundamentals/README.md)**
