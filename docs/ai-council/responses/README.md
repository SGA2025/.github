# Responses Directory

Each file here is a verbatim response from a single AI to a single
prompt. Files are immutable after commit.

## File Naming

```
YYYY-MM-DD_ai-name_topic.md
```

Examples:

- `2026-05-10_perplexity_v1-review.md`
- `2026-05-10_gemini-1_v1-review.md`
- `2026-05-10_gemini-2_v1-review.md`
- `2026-05-10_gpt_v1-review.md`

When the same AI responds twice on the same date, append a sequence
number after the AI name (e.g., `gemini-1`, `gemini-2`).

## Recommended Response File Template

```markdown
# Response from <AI name> on <topic>

**Date**: YYYY-MM-DD
**Prompt source**: ../prompts/<filename>.md
**AI version / model**: e.g., ChatGPT (GPT 5.x Thinking), Gemini Advanced, Perplexity Pro
**Format**: verbatim | abridged | translated
**Operator note**: any context the operator wants to add (e.g., the AI
was asked a follow-up question that produced this response)

---

[Body of response, unmodified except for redaction noted below.]

---

## Redactions

- None.

(or list any redactions made for confidentiality, with rationale.)
```

## Immutability

Once a response is committed, do not edit the body. If the responding AI
issues a correction, add a new file with the same date and the
`_corrigendum` suffix:

```
2026-05-10_gpt_v1-review.md             # original
2026-05-10_gpt_v1-review_corrigendum.md  # later correction
```

## What Goes Here vs. `reference/`

- **`responses/`** — first-party AI responses to prompts in this workspace.
- **`reference/`** — external materials, third-party documents, or AI
  commentary that arrived outside the prompt/response cycle (e.g., an AI
  comments on a draft framework that was not yet a formal prompt).

## Confidentiality

Responses MUST NOT contain client identifiers, case numbers, privileged
content, or secrets. If a reviewer AI inadvertently includes such content
in its response, the operator removes the file before commit and may
document the incident in an internal log (outside this repository).
