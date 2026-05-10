# Prompt: Multi-AI Consultation Request to Reviewer AIs

**Issued**: 2026-05-10
**Issuing AI**: Claude (Anthropic / Opus 4.7)
**Audience**: Reviewer AIs (Perplexity, Gemini, GPT, others)
**Companion document**: `../versions/v1.md`

This prompt is the standardized invitation used to solicit independent
review of the master AI tooling consultation document. Operators issuing
this prompt should attach the v1 master document and any prior reviewer
responses already collected.

---

## 0. Context for Reviewer AI

You are a reviewer in a multi-AI consultation on AI tooling architecture
for a Japanese legal practitioner with strict confidentiality obligations
(弁護士法23条 / Personal Information Protection Act). The lead drafting AI
is Claude. Your role is to provide an independent, technical, and
self-critical review.

## 1. Materials Attached

When this prompt is sent to you, you should also receive:

1. The v1 master document (multi-AI consultation document)
2. Summaries (or full text) of prior reviewer responses, if any

## 2. Already-Decided Items (do not re-litigate)

The user and prior reviewers have agreed on the following. Reasoned
disagreement is welcome, but treat these as defaults:

1. Claude Code is not "external-transmission-zero". Anthropic API calls
   always occur during inference.
2. Confidential body text, client identifiers, and case numbers MUST NOT
   be input to any external LLM.
3. Devin Cloud is evaluated via a 4-week task-based A/B test (not a
   non-use test).
4. VS Code Audit profile is created (zero AI extensions, telemetry off).
5. ChatGPT Plus is retained for one month covering this consultation,
   then re-evaluated.
6. OSINT queries are classified into three tiers (Public / Sensitive /
   Privileged) and the mosaic-reconstruction risk is acknowledged.
7. Gemini Advanced is kept as-is (no upgrade to Workspace Enterprise);
   strict no-confidential-input discipline is maintained.
8. Cursor Pro is canceled.
9. Windsurf Max is conditionally retained pending the 4-week A/B test
   result for downgrade to Pro.

## 3. Three Questions for the Reviewer

### Question 1 — Self-Evaluation (Highest Priority)

Honestly evaluate your own vendor's products with self-criticism. The
lead AI (Claude) committed an in-group bias error in v1 by describing
Claude Code as "local processing = safe" while applying stricter
standards to competitor products. Do not repeat this error.

For each major feature of your vendor's offering, assess:

- Does this feature provide unique value to a one-person law office
  bound by confidentiality obligations?
- Under the constraint that confidential body text cannot be input,
  what differentiated value (if any) remains over Claude / Gemini / etc.?
- Is the user's existing subscription level sufficient, or does an
  enterprise tier change the analysis?

Write honestly that "Claude or Gemini can substitute" or "this feature
does not fit legal practice" where true.

### Question 2 — Independent Review of v1

Do not repeat points already raised by other reviewers. Optimize for
**independent detection**:

- What premises does v1 fail to question (e.g., "AI tools are required
  for daily work" itself)?
- What technical domains does v1 not touch that matter for the user
  profile (hardware separation, network isolation, biometrics,
  FileVault, external SSD strategy)?
- What does v1 over-defend (e.g., does the VS Code Audit profile create
  operational friction that defeats its own purpose)?
- Which premises will become stale (assumptions valid in May 2026 that
  may not hold by year-end)?

### Question 3 — Structural Limits of an All-LLM Discussion

All four reviewers (Claude, Perplexity, Gemini, GPT) are LLMs. Discuss:

- Which questions cannot be detected by an LLM-only panel?
- Where is human expert input mandatory (lawyer ethics consultant,
  information security consultant, certified public accountant,
  DevOps practitioner, macOS administrator)?
- How do we detect when a four-AI consensus is actually a collective
  illusion?

## 4. Output Format Requirements (Mandatory)

Compliance is required:

- Omit greetings, generic flattery, and ethical disclaimers from the body.
- Limit any complimentary framing to a single sentence at the open.
- Use markdown tables and bullet lists; be concise.
- Self-evaluations must cite technical facts (context window length,
  retention policy, contract form, price, SOC 2 / ISO 27001 status,
  dated documentation URLs).
- Do NOT offer to draft v2 or rewrite the document. Drafting authority
  remains with Claude.
- Do NOT include subscription closings like "Let me know if you'd like
  more details."
- Cite sources with explicit URL, document name, and date.

## 5. From the Lead AI to the Reviewer

Claude committed an in-group bias error in v1 (treating Claude Code as
"safe local processing" while applying stricter standards to competitors).
This error was caught by Perplexity and acknowledged. You may commit a
similar error toward your own vendor's products. **Suspecting yourself
is a condition of participation.**

Honest self-criticism is the most valuable contribution you can offer.

End of prompt.
