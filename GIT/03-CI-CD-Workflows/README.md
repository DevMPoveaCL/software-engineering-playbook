# 🔄 CI/CD Workflows

> **Why this matters:** CI/CD is the assembly line that takes your code from "done" to "deployed" automatically — no human manually clicking buttons.

---

## 🧠 Mental Model: The Restaurant Assembly Line

Imagine your code is a **dish** being prepared in a professional kitchen:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   RAW INGREDIENTS          KITCHEN PROCESS           SERVED    │
│   (Your Code)                   │                     DISH     │
│       │                         │                       ▲      │
│       ▼                         ▼                       │      │
│   ┌───────┐    ┌─────────┐    ┌────────┐    ┌────────┐ │      │
│   │CLEAN  │───▶│ PREPARE │───▶│  COOK  │───▶│PLATEUP │─┘      │
│   │  AND  │    │(Measure)│    │(Heat)  │    │(Garnish)│        │
│   │ CHECK │    │         │    │        │    │         │        │
│   └───────┘    └─────────┘    └────────┘    └────────┘        │
│       │                                                      │
│       ▼                                                      │
│   INGREDIENT                                           FINAL  │
│   INSPECTION                                           PRODUCT │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**CI/CD is that assembly line for software:**
- **Continuous Integration (CI):** Every "ingredient" (code change) gets tested and validated automatically
- **Continuous Deployment (CD):** Once validated, the dish is automatically "served" (deployed) to customers

---

## 🔄 Continuous Integration (CI)

### What It Does

CI runs **every time you push code** (or open a PR):

1. **Build** — Compile/bundle your code
2. **Test** — Run all automated tests
3. **Validate** — Check code quality, security, style

### ASCII: CI Pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   YOU PUSH CODE                                                 │
│         │                                                        │
│         ▼                                                        │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │              CI SERVER (GitHub Actions, etc.)             │   │
│   │                                                          │   │
│   │   ┌─────────┐                                            │   │
│   │   │  BUILD  │ ─── "Can it compile?"                      │   │
│   │   └────┬────┘                                            │   │
│   │        │                                                  │   │
│   │        ▼                                                  │   │
│   │   ┌─────────┐                                            │   │
│   │   │  TEST   │ ─── "Do the tests pass?"                   │   │
│   │   └────┬────┘                                            │   │
│   │        │                                                  │   │
│   │        ▼                                                  │   │
│   │   ┌─────────┐                                            │   │
│   │   │ LINT +  │ ─── "Is it clean code?"                    │   │
│   │   │ QUALITY │                                            │   │
│   │   └────┬────┘                                            │   │
│   │        │                                                  │   │
│   │        ▼                                                  │   │
│   │   ┌─────────┐                                            │   │
│   │   │ SECURITY│ ─── "Any secrets or vulnerabilities?"     │   │
│   │   │  SCAN   │                                            │   │
│   │   └────┬────┘                                            │   │
│   │        │                                                  │   │
│   │        ▼                                                  │   │
│   │   ┌─────────┐                                            │   │
│   │   │REPORT   │ ◄── "Here's what happened"               │   │
│   │   └─────────┘                                            │   │
│   │                                                          │   │
│   └─────────────────────────────────────────────────────────┘   │
│         │                                                        │
│         ▼                                                        │
│   ┌─────────────┐     ┌─────────────┐                           │
│   │  ✅ GREEN   │     │   ❌ RED     │                           │
│   │  All good!  │     │  Something  │                           │
│   │  Safe to    │     │  failed!    │                           │
│   │  proceed    │     │  Fix before  │                           │
│   └─────────────┘     │  merging    │                           │
│                        └─────────────┘                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Continuous Deployment (CD)

### What It Does

CD takes the validated code and **automatically deploys** it:

1. **Stage** → Deploys to staging environment
2. **Verify** → Runs smoke tests against staging
3. **Production** → Deploys to production (or needs manual approval)

### ASCII: CD Pipeline (Full Flow)

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   COMMIT PASSES CI                                               │
│         │                                                        │
│         ▼                                                        │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────────────┐ │
│   │   BUILD     │───▶│   TEST      │───▶│   DEPLOY STAGING    │ │
│   │  (Compile)  │    │  (Unit/E2E) │    │   (Auto)            │ │
│   └─────────────┘    └─────────────┘    └──────────┬──────────┘ │
│                                                       │           │
│                                                       ▼           │
│                                              ┌─────────────┐      │
│                                              │ SMOKE TEST  │      │
│                                              │ (Does it    │      │
│                                              │  actually   │      │
│                                              │  work?)     │      │
│                                              └──────┬──────┘      │
│                                                     │            │
│            ┌───────────────────────────────────────┘             │
│            ▼                                                       │
│   ┌─────────────────┐        ┌─────────────────┐                 │
│   │ AUTO DEPLOY     │        │  WAIT FOR       │                 │
│   │ TO PRODUCTION   │        │  MANUAL APPROVAL│                 │
│   │ (GitOps style)  │        │  (Sensitive     │                 │
│   └─────────────────┘        │   deployments)  │                 │
│                              └─────────────────┘                 │
│                                    │                             │
│                                    ▼                             │
│                              ┌───────────┐                       │
│                              │ 🎉 LIVE!  │                       │
│                              │ Customers │                       │
│                              │   using   │                       │
│                              │   it now  │                       │
│                              └───────────┘                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Typical CI/CD Workflows

### Workflow 1: Simple (GitHub Actions)

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run linter
        run: npm run lint
        
      - name: Run tests
        run: npm test
        
      - name: Build
        run: npm run build
```

### Workflow 2: With Deployment

```yaml
# Deploys to staging automatically, production on release tag
name: Deploy

on:
  push:
    branches: [main]
  release:
    types: [published]

jobs:
  deploy-staging:
    if: github.event_name == 'push'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: ./deploy.sh staging
      # Deploys automatically after CI passes

  deploy-production:
    if: github.event_name == 'release'
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      - name: Require approval
        run: echo "Waiting for manual approval"
      - run: ./deploy.sh production
```

---

## ⏱️ CI/CD Before/During/After

### ❌ Before CI/CD (The Dark Ages)

```bash
# Manual, error-prone deployment
1. Developer finishes feature
2. Developer manually runs tests locally
3. Developer manually packages application
4. Developer SFTPs to server (with credentials!)
5. Developer restarts service
6. Developer hopes nothing breaks
7. No rollback plan if it does break
```

**Problems:**
- Human error is inevitable
- No standardization = snowflake servers
- Credentials scattered
- No audit trail
- Rollback = panic

### 🔄 During CI/CD (The Modern Way)

```bash
# Automated, reliable deployment
1. Developer finishes feature
2. Developer opens PR
3. CI runs: lint + test + build automatically
4. Reviewer approves PR
5. PR merged to main
6. CD triggers: build + deploy to staging
7. Smoke tests run against staging
8. If green: deploy to production (or auto or manual gate)
9. Full audit trail in GitHub
10. One-click rollback if needed
```

**Benefits:**
- Standardized every time
- No credentials on developer machines
- Full audit trail
- Rollback = click a button
- Consistency across environments

### 🎉 After CI/CD (The Ideal State)

```bash
# Developers focus on CODE, not deployment
1. Write feature
2. Open PR
3. Get review
4. Merge
5. 🚀 AUTOMATICALLY deployed to production within minutes
6. Monitor metrics
7. If issues: rollback with one click
```

---

## 🛡️ CI/CD Security Practices

| Practice | Why |
|----------|-----|
| **Secrets in vault** | Never commit credentials; use GitHub Secrets or Vault |
| **Dependency scanning** | Catch vulnerable libraries before they ship |
| **SAST (Static Analysis)** | Scan code for security flaws automatically |
| **Container scanning** | Ensure Docker images don't have vulnerabilities |
| **Environment isolation** | Staging ≠ Production credentials |

---

## ✅ What TO Do

| Do This | Why |
|---------|-----|
| **Fail fast** | CI should catch issues in minutes, not days |
| **Make tests deterministic** | Flaky tests = no trust in CI |
| **Keep CI under 10 minutes** | Slow CI = developers skip it mentally |
| **Require CI to pass before merge** | No exceptions — ever |
| **Use the same process for all envs** | Eliminate "works on my machine" |
| **One-click rollback** | If production breaks, fix takes seconds |

---

## 🚫 What NOT To Do

| Don't Do This | Why Not |
|---------------|---------|
| **Don't skip CI when in a hurry** | Haste + no net = production outage |
| **Don't hardcode credentials** | They're in the repo forever |
| **Don't deploy from local machine** | Creates snowflake servers |
| **Don't skip staging** | Staging catches what dev missed |
| **Don't disable failing tests** | Technical debt that compounds |
| **Don't deploy on Fridays** | You're gambling with your weekend |

---

## 🔧 Common CI/CD Platforms

| Platform | Best For | Notes |
|----------|----------|-------|
| **GitHub Actions** | GitHub-hosted projects | Tight integration, free for open source |
| **GitLab CI/CD** | GitLab repos | Very powerful, YAML-based |
| **Jenkins** | Enterprise, self-hosted | Flexible but needs maintenance |
| **CircleCI** | Speed | Fast, good caching |
| **ArgoCD** | GitOps, Kubernetes | Declarative, follows git state |

---

[⬅️ Back to Parent](../README.md)
