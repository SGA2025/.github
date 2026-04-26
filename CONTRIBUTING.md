# Contributing to SGA2025 Repositories

Thank you for your interest. This document describes the
**multi-party protocol** used for all changes across SGA2025
repositories.

The protocol was developed during the IPFA P0 remediation
engagement (2026-04-26) and the security infrastructure setup
that followed. See the IPFA repository for the original
engagement report.

---

## Multi-Party Protocol

| Party | Role | Authority |
|-------|------|-----------|
| **Owner** | Decision authority, physical device testing, final merge | Sole merge gatekeeper |
| **Cascade** (Windsurf IDE) | Implementation in IDE, deep code review, protocol guardian | Approval blocker if spec violated |
| **Devin** (Cognition AI) | Implementation for application code, automated testing (Playwright), PR creation | Cannot self-approve or merge |
| **Comet** (Perplexity) | Independent second reviewer, GitHub UI operator (under supervision) | Cannot self-approve or merge |

### Why multiple parties

- **Devin alone** lacks physical iOS device testing and can
  produce scope creep without external review
- **Owner alone** lacks bandwidth for line-by-line code review
- **Cascade alone** lacks implementation throughput
- **Comet alone** lacks deep code context
- **Together**: complementary capabilities, mutual verification,
  and scope discipline

### Tool selection by task type

| Task type | Best tool |
|-----------|-----------|
| Application code (HTML/CSS/JS) | Devin |
| Infrastructure (.github/, configs) | Cascade |
| Independent PR review | Comet (or Cascade for deep IDE review) |
| GitHub UI operations | Comet (browser-native) |
| Architecture decisions | Owner (sometimes assisted by Cascade) |
| Documentation drafting | Cascade or Devin |

### Complementary Review Roles

When Cascade and Devin both review a change, their focus areas should
**complement, not duplicate**:

| Reviewer | Primary focus | Strength |
|----------|---------------|----------|
| **Devin** | Architectural correctness, security implications, whether config achieves its stated goal | Independent reasoning from first principles |
| **Cascade** | Literal plan compliance, field-by-field diff against spec, deviations from documented decisions | Knows the plan line-by-line (often as author) |

Why this matters:

- A reviewer is **more likely** to catch deviations that **contradict** their own reasoning
- A reviewer is **less likely** to catch deviations that **align** with their own reasoning (cognitive framing bias)
- Two reviewers with different mental models catch different deviations; neither alone is sufficient

**Reference case (2026-04-26):** branch protection review on PR #13.

- Devin caught `required_approving_review_count: 1` deviation via security reasoning ("bypass-by-default normalizes bypassing")
- Cascade caught `enforce_admins: false` deviation via literal plan re-read (plan said `✅ Yes`)
- Devin's own prior recommendation (D2: solo account needs flexibility) had created a cognitive frame that made `enforce_admins: false` look intentional rather than erroneous
- Cascade's plan re-read, untainted by D2 reasoning, surfaced it

Neither reviewer alone would have caught both. Both were necessary.

---

## Workflow per Change

### 1. AUDIT-driven planning (for non-trivial changes)

Before any implementation:
- Owner publishes an AUDIT document if scope is unclear
- AUDIT must specify: goals, hard constraints, allowed/forbidden
  files, risk classification, PR strategy
- See `AUDIT-2026-04-26.md` (in ifpa-website) as a reference template

### 2. Implementation

- Implementer (Devin/Cascade) works on a **new branch** from
  latest `main`
- Branch name format: `<role>/<short-description>`
  (e.g., `devin/fix-contact-form`, `cascade/security-infrastructure`)
- **One PR per issue** — do not bundle unrelated changes
- Implementer produces a **work summary** documenting:
  - Files changed and rationale
  - Verification steps performed
  - Risks and mitigations
  - Rollback procedure

### 3. Code review

- **Cascade** does deep IDE-side review (line-by-line, regression
  checks, spec compliance)
- **Comet** does independent second review against the security
  self-check (see PR template, 6-point Comet checklist below)
- Both produce written findings before any merge

### 4. Physical device testing (Owner)

For any mobile-affecting change, Owner tests on a physical iPhone
via local HTTP server (see ENGAGEMENT-REPORT-2026-04-26.md
Section 7 in ifpa-website).

For low-risk changes (pure deletion, content edits), Mac Safari
responsive design mode may suffice.

### 5. Multi-party written approval

All relevant parties confirm in writing before merge:
- Owner: "Tested, approved to merge."
- Cascade: "Diff reviewed, spec-compliant."
- Devin (if implementer): "Implementation complete, CI green."
- Comet (if reviewer): "Independent review confirms scope and safety."

### 6. Owner merges

- Only the Owner clicks **Merge** on GitHub
- Squash-and-merge is preferred for clean history
- After merge, verify production deployment within 5 minutes

### 7. GitHub Actions Workflow Trial-Run Requirement

For any change that adds or modifies a `.github/workflows/*.yml` file,
this additional verification is **required** before declaring review
feedback "applied" or "verified":

1. **Push to a feature branch** (NOT `main`; let PR-trigger or push-trigger fire the workflow)
2. **Wait for the workflow run to complete**
3. **Document the run URL and per-step conclusions** in the PR description or review log
4. **For expected failures** (e.g., GHAS-dependent steps on private repos), **predict the failure mode in writing BEFORE the trial run completes**, then confirm the actual failure matches the prediction

**A workflow change is "verified" only after runtime evidence, not after
string-level review.**

#### Why Step 4 matters

Step 4 turns "expected failures" into **testable predictions** rather
than **post-hoc excuses**:

- ✅ Predicted: "Perform CodeQL Analysis will fail with 'code scanning not enabled' on private repo"
- ✅ Actual: matches prediction → workflow is correct; failure is environmental
- ❌ Without Step 4: any failure can be retroactively labeled "expected" without rigor

#### Reference case (2026-04-27)

PR #13's workflow had `queries: default`, which was string-reviewed
against Devin's D3 spec recommendation by **four review layers**:

- Round 1: Devin's own spec recommendation matched the YAML literal
- Round 4: Cascade's self-review matched recommendation to YAML
- Round 6: Devin's PR review matched YAML to recommendation
- Round 6: Cascade's plan re-read matched both to plan text

All four passed. The runtime caught the bug in 30 seconds: `default` is
not a valid CodeQL query pack name. Adopting this trial-run rule
prevents that class of escape.

---

## Risk Classification

Every PR must be classified as Green, Yellow, or Red:

| Level | Examples | Process |
|-------|----------|---------|
| 🟢 **Green** | Copy edits, color tweaks, image swaps, static section additions | Standard review; Mac Safari responsive mode sufficient |
| 🟡 **Yellow** | Routes, forms, SEO metadata, dependencies, build config, navigation, inline-CSS cleanup | Standard review + physical iPhone test (only if mobile-affecting) |
| 🔴 **Red** | DNS, payments, login, database, secrets, GitHub Actions, deployment settings | **No automation** — Owner executes manually with Devin/Cascade providing only the plan |

---

## Hard Constraints (apply to all SGA2025 repos)

### Forbidden
- ❌ Direct push to `main` (enforced by branch protection)
- ❌ API keys, tokens, `.env` content, or secrets in code/logs
- ❌ DNS, domain registrar, payment, database modifications
  without separate Owner approval
- ❌ Customer information, internal URLs, private emails

### Required
- ✅ All new files documented (purpose stated in PR)
- ✅ Rollback plan in PR description
- ✅ Mobile and desktop both verified (for user-facing changes)

### Repository-specific constraints

Some repos have additional constraints in their own CONTRIBUTING.md:
- **ifpa-website:** No npm, no build tools, no third-party services
  (must remain pure static HTML/CSS/JS)

---

## Comet 6-Point Review Checklist

When Comet reviews a PR, it should verify:

1. ☐ Are file changes within scope (not exceeding the task)?
2. ☐ Any new dependencies, scripts, external CDNs, tracking
   code introduced?
3. ☐ Are .github/, deployment, DNS, env, config changes
   appropriate and justified?
4. ☐ Could the changes affect mobile, SEO, accessibility,
   language routing?
5. ☐ Any hardcoded secrets, internal URLs, customer
   information, or private emails?
6. ☐ Can this PR be reverted with a single commit?

---

## Devin Engagement Template

When engaging Devin for new work:

```
Please work only on a new branch; do NOT modify main/production.

Goal: <one-line description>

Allowed files: <explicit paths>

Forbidden files:
- package.json, package-lock.json (do not introduce npm)
- .github/workflows/ (CI changes require separate PR)
- DNS, env, deployment config

Risk classification: 🟢 Green / 🟡 Yellow / 🔴 Red

Acceptance criteria:
1. <specific requirement>
2. No console errors on all pages
3. Both JP and EN versions updated (if applicable)
4. Rollback feasible via single git revert

After completion, please provide:
- File-by-file change list with rationale
- Verification steps performed
- Self-check against PR template security items
- Rollback instructions
```

---

## Engagement History

For the most current engagement history, see the relevant
repository's `ENGAGEMENT-REPORT-*.md` files.

Major engagements:
- **2026-04-26**: IPFA P0 remediation (3 PRs) — see
  `ifpa-website/ENGAGEMENT-REPORT-2026-04-26.md`
- **2026-04-26**: SGA2025 security infrastructure baseline —
  see `ifpa-website/SECURITY-INFRASTRUCTURE-PLAN-2026-04-26.md`

---

## Questions

For questions about contributing, contact `info@ipfa.info`.
For security concerns, see `SECURITY.md`.
