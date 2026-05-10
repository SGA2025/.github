# AGENTS.md — `docs/ai-council/` Workspace Working Rules

This file governs every AI agent operating inside `docs/ai-council/`.
It is the local override of the parent repository's instructions for
this workspace only.

## 1. Purpose

This workspace coordinates multi-AI review of policy and architecture
documents (currently the AI tooling stack consultation for a Japanese
legal practitioner). Its design goals are:

- An immutable per-AI record of contributions
- Clear separation between drafting authority and reviewer role
- A reusable scaffold for inviting additional AIs

## 2. Directory Structure

| Directory | Purpose | Editable after commit? |
|---|---|---|
| `prompts/` | Instruction templates sent to AIs for review | No |
| `responses/` | Verbatim per-AI responses to prompts | No |
| `reference/` | External materials and AI commentary received outside the prompt/response cycle | No |
| `versions/` (created when needed) | Successive versions of master documents (v1, v2, ...) | New file per version |

## 3. File Naming Convention

All files under `prompts/`, `responses/`, and `reference/` use:

```
YYYY-MM-DD_ai-name_topic.md
```

- `YYYY-MM-DD` is the date of the response (or prompt issuance), not the commit date.
- `ai-name` is lowercase: `claude`, `perplexity`, `gemini`, `gpt`, etc.
- `topic` is short, hyphen-separated, descriptive.

Examples:
- `prompts/2026-05-10_to-other-ai.md`
- `responses/2026-05-10_gpt_v1-review.md`
- `reference/2026-05-10_gpt_v2-framework-comment.md`

## 4. Immutability Rule

Files under `prompts/`, `responses/`, and `reference/` MUST NOT be edited
after commit. Corrections are made by adding a new file with the
`_corrigendum` suffix:

```
2026-05-10_gpt_v1-review.md           (original)
2026-05-10_gpt_v1-review_corrigendum.md  (correction, added later)
```

Master documents under `versions/` follow the same rule: do not edit `v1`
to make `v2`; create a new file `v2.md`.

## 5. Drafting Authority

1. **Claude** is the lead drafting AI for master documents in this workspace.
2. Other AIs (Perplexity, Gemini, GPT, etc.) act as **reviewers only**.
3. Reviewer AIs MUST NOT unilaterally rewrite master documents. Their
   contribution is delivered as a `responses/` file. Integration into the
   next master document version is performed by the lead AI.
4. Offers like "Shall I rewrite v2?" or "I will produce the next version"
   from reviewer AIs are declined per this rule.
5. The user (operator) retains final approval authority on every commit.

## 6. Response Format Requirements

When this workspace solicits an AI response, the AI MUST:

- Omit greetings, generic flattery, and boilerplate disclaimers.
- Use markdown tables and bullets for structured content.
- Cite technical facts (retention policy, context length, contract type,
  price, certifications, dated documentation URLs) for self-evaluations.
- Disclose self-criticism when evaluating its own vendor's products.
- Refrain from offering to take over drafting (see Section 5).

## 7. Confidentiality

This workspace is a documentation surface. It MUST NOT contain:

- Client identifiers, case numbers, or names of opposing parties
- Privileged communications or attorney work product
- Personal data of identifiable individuals
- Credentials, tokens, or secrets

Documents describe AI architecture decisions only. If sensitive content
slips into a review, the file MUST be removed immediately and the
incident logged outside this workspace.

## 8. Onboarding a New Reviewer AI

1. Reviewer reads this file (`AGENTS.md`).
2. Reviewer reads the latest prompt in `prompts/`.
3. Reviewer's response is saved to `responses/` by the operator.
4. The lead AI integrates the response in the next master document version.

## 9. Inheritance

This file inherits from the parent repository's `AGENTS.md` and `CLAUDE.md`
where present, and overrides them only within `docs/ai-council/`.
