# Session Handoff: Round 2 Integration Task

**Date**: 2026-05-10
**From**: Claude session (`claude/unified-agent-rules-qlzyT`,
last commit `f8392c8`) operating with `Allowed repositories: sga2025/.github` only
**To**: Next Claude session, expected to be launched with
`Allowed repositories: sga2025/.github + sga2025/derisk`
**Path**: `docs/ai-council/reference/2026-05-10_session-handoff.md`

## Purpose

This file is a self-contained handoff so the next Claude session can
resume the coordinator role without depending on conversation history
transfer. Reading this file plus the files it references is sufficient
to pick up the work.

## Operator's Standing Directives

1. **Claude is the council coordinator (まとめ役)** by operator's
   appointment 2026-05-10.
2. **Each AI writes only to its own workspace**. Cross-repo writing is
   forbidden. `sga2025/.github` is Claude's and Cascade's; `sga2025/derisk`
   is Codex's.
3. **No file in `responses/` or `reference/` may be edited after commit**
   (AGENTS.md §4 immutability). Corrections take the form of a new
   `_corrigendum_NN` file or a new dated revision file.
4. **The operator retains final approval** on every commit and may
   override anything without explanation.

## State at Handoff

### `sga2025/.github`, branch `claude/unified-agent-rules-qlzyT`

Already committed (immutable):

| Path | Commit | Author | Type |
|---|---|---|---|
| `docs/ai-council/AGENTS.md` | `08cd29c` | Claude | protocol |
| `docs/ai-council/README.md` | `08cd29c` | Claude | protocol |
| `docs/ai-council/responses/README.md` | `08cd29c` | Claude | protocol |
| `docs/ai-council/prompts/2026-05-10_to-other-ai.md` | `08cd29c` | Claude | R1 prompt |
| `docs/ai-council/responses/2026-05-10_perplexity-1_v1-review.md` | `6c3e6fb` | Perplexity | R1 |
| `docs/ai-council/responses/2026-05-10_perplexity-2_meta-on-gemini.md` | `6c3e6fb` | Perplexity | R1 |
| `docs/ai-council/responses/2026-05-10_gemini-1_v1-review.md` | `6c3e6fb` | Gemini | R1 |
| `docs/ai-council/responses/2026-05-10_gemini-2_post-perplexity.md` | `6c3e6fb` | Gemini | R1 |
| `docs/ai-council/reference/2026-05-10_gpt_v2-framework-comment.md` | `6c3e6fb` | ChatGPT | R1 |
| `docs/ai-council/versions/v1.md` | `a534e99` | Claude | master v1 |
| `docs/ai-council/reference/2026-05-10_claude_post-reviews-synthesis.md` | `c1e7b15` | Claude | R1 synthesis |
| `docs/ai-council/versions/v2-framework-draft.md` | `f8392c8` | Claude | R2 framework draft |

### `sga2025/derisk`, branch `codex/add-gpt-review-request-packet`

**Not accessible from the previous session.** Confirmed via
`mcp__github__search_code` that the repository is private and contains
at least these files (paths reported by operator):

1. `docs/ai-council/README.md`
2. `docs/ai-council/AGENTS.md`
3. `docs/ai-council/prompts/2026-05-10_to-other-ai.md` (Codex's version)
4. `docs/ai-council/reference/2026-05-10_gpt_v2-framework-comment.md` (Codex variant)
5. `docs/ai-council/responses/2026-05-10_perplexity_v2-framework-review.md`
6. `docs/ai-council/responses/2026-05-10_gemini_repo-architecture-advice.md`

(Files 1–4 confirmed via search index. Files 5–6 reported by operator
but not yet visible in search index, possibly because the codex branch
is non-default and search lag.)

## Task for the New Session

### Step 1. Verify derisk access

Try this immediately:

```
mcp__github__get_file_contents(
  owner="SGA2025",
  repo="derisk",
  path="docs/ai-council/AGENTS.md",
  ref="refs/heads/codex/add-gpt-review-request-packet"
)
```

**Expected**: file contents returned.
**If denied**: report to operator that derisk allowlist update did not
take effect; do not proceed with steps 2–6.

### Step 2. Read all 6 derisk files

In order of priority for understanding:

1. `AGENTS.md` and `README.md` — protocol context
2. `prompts/2026-05-10_to-other-ai.md` — Codex's prompt, diff vs ours
3. `responses/2026-05-10_perplexity_v2-framework-review.md` — Perplexity R2
4. `responses/2026-05-10_gemini_repo-architecture-advice.md` — Gemini R2
5. `reference/2026-05-10_gpt_v2-framework-comment.md` — Codex variant of GPT comment

### Step 3. Archive R2 responses in `sga2025/.github`

Per immutability rules, create new files (do not edit existing ones).
Suggested filenames:

- `docs/ai-council/responses/2026-05-10_perplexity_v2-framework-review_from-derisk.md`
- `docs/ai-council/responses/2026-05-10_gemini_repo-architecture-advice_from-derisk.md`
- `docs/ai-council/reference/2026-05-10_gpt_v2-framework-comment_from-derisk.md`
- `docs/ai-council/reference/2026-05-10_codex_AGENTS-from-derisk.md` (if Codex's AGENTS.md differs from ours)
- `docs/ai-council/reference/2026-05-10_codex_README-from-derisk.md` (if Codex's README differs)
- `docs/ai-council/reference/2026-05-10_codex_prompt-from-derisk.md` (if Codex's prompt differs)

Each archived file should have a header noting:
- Source: `sga2025/derisk` branch `codex/add-gpt-review-request-packet`,
  full path, commit SHA at time of read
- Author per operator's attribution
- Verbatim copy

### Step 4. Produce synthesis r2

File: `docs/ai-council/reference/2026-05-1X_claude_synthesis-r2.md`
(use today's date; if 2026-05-10 still, suffix with `_v2` or `_b`).

Should include:
- Cross-AI consensus matrix (extend the R1 matrix from
  `2026-05-10_claude_post-reviews-synthesis.md`)
- New points raised in R2
- Points where R2 contradicts R1
- Updated answers to OQ-1 through OQ-5
- Items confirmed for v2 (revised list)

### Step 5. Produce framework rev2

File: `docs/ai-council/versions/v2-framework-draft-rev2.md` (do NOT
overwrite `v2-framework-draft.md` — immutability extended to drafts per
the framework's §5).

Should include:
- 10-section outline updated per R2 input
- Per-section attribution of which AI contributed which idea
- Open issues remaining

### Step 6. Coordinator report to operator

Brief textual report (in chat, not a file) summarizing:
- What changed between R1 and R2
- Decisions that need operator approval
- Recommended next step (typically: produce final v2.md)

## Constraints to Maintain

- **Do not write to `sga2025/derisk`**, even after access is granted.
  Codex's workspace remains Codex's.
- **Do not edit any existing file** in `responses/`, `reference/`, or
  `versions/` of `sga2025/.github`. Always create a new file.
- **Do not amend commits.** Always create new commits.
- **Branch**: `claude/unified-agent-rules-qlzyT`. Push with
  `git push -u origin claude/unified-agent-rules-qlzyT`.
- **Commit message footer**: include `https://claude.ai/code/...`
  attribution.

## Consulting Pattern Going Forward

For future rounds (R3, R4, ...):

1. Coordinator (Claude) drafts a new prompt or framework revision in
   `prompts/` or `versions/`.
2. Each AI reads it from their accessible vantage point (Cascade and
   Claude can read .github directly; Codex via cross-repo if granted, or
   via operator relay).
3. Each AI writes their response in their own workspace.
4. Coordinator reads all responses (with derisk access if granted),
   produces a new synthesis and revised framework as new files.
5. Operator approves before any final master document version
   (`versions/vN.md`) is committed.

## Open Issues Carried Forward

| ID | Question | Status |
|---|---|---|
| OQ-1 | Is "GitHub repository" the right unit of separation, or should "credential scope" be elevated? | Open, pending R2 |
| OQ-2 | Is the 4-week Devin A/B test design sufficient to isolate marginal value? | Open, pending R2 |
| OQ-3 | Is "ritualization fatigue" of audit profiles GPT-unique or observed elsewhere? | Open, pending R2 |
| OQ-4 | Should v2 explicitly declare the single-operator sizing premise? | Open, pending R2 |
| OQ-5 | How aggressively should v2 abstract away model names, and at what UX cost? | Open, pending R2 |

End of file.
