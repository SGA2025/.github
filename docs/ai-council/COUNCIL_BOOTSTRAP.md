---
title: "AI Council Bootstrap Protocol"
version: 1.1
date: 2026-05-10
changelog:
  - "v1.1 (2026-05-10): canonical_paths updated for derisk branch migration to claude/council-clean-2026-05-10. §2.1 updated with branch consolidation status. §11 deployment URLs updated."
  - "v1.0 (2026-05-10): initial release."
applies_to:
  - Claude Code (Anthropic)
  - Cascade (Windsurf / Codeium)
  - GPT (OpenAI / Codex / ChatGPT)
  - Gemini (Google)
  - Perplexity (Computer)
  - Devin (Cognition, future)
load_order: "SESSION_START — read this file BEFORE any council-related work"
canonical_paths:
  - sga2025/.github @ claude/unified-agent-rules-qlzyT : docs/ai-council/COUNCIL_BOOTSTRAP.md
  - sga2025/derisk @ claude/council-clean-2026-05-10 : docs/ai-council/COUNCIL_BOOTSTRAP.md
mirror_status: "Two files, identical content. Update both atomically."
upstream_authority: "AGENTS.md @ derisk/claude/council-clean-2026-05-10 (commit 22a202b8 or later)"
edit_policy: "This file is reference/bootstrap. Modify only via PR with coordinator (Claude) review + human owner approval."
---

# AI Council Bootstrap Protocol

> このファイルは Multi-AI Council に参加する各 AI が **session 開始時に必ず読む** ブートストラップ手順書。
> AI を新しいセッションで起動した直後、council 関連の作業（読み・書き・引用・反論）に着手する**前**に、本書 §0 の checklist を完了すること。

---

## §0. Mandatory Session Start Checklist

各 AI は council 作業に着手する**前**に、以下 5 ステップを順に完了する。

```
[ ] Step 1: Read AGENTS.md (canonical: derisk/claude/council-clean-2026-05-10 @ HEAD)
[ ] Step 2: Read this COUNCIL_BOOTSTRAP.md (you are here)
[ ] Step 3: Read latest v2.md (versions/v2.md, both repos)
[ ] Step 4: Read latest coordinator synthesis
            (responses/2026-05-10_claude_coordinator-synthesis-and-handoff-reply.md)
[ ] Step 5: Self-test — answer the 5 questions in §8 in your scratch space
            (do not commit answers; they are integrity checks for you)
```

完了するまで、reference/ または versions/ の参照をしない。responses/ への新規ファイル投稿もしない。

---

## §1. Role Routing

| AI | 主担当領域 | 書込先 (responses/) | 役割タイプ |
|---|---|---|---|
| **Claude Code** (Anthropic) | coordinator、横断 synthesis、handoff 統合 | 両 repo | coordinator + reviewer |
| **Cascade** (Codeium / Windsurf) | 実行系、IDE 統合、git 操作、実環境テスト、別紙 A 起案 | derisk @ claude/council-clean-2026-05-10 | reviewer + executor + drafter |
| **GPT** (OpenAI / Codex) | 構造提案、リファクタ提案、council reviewer | derisk @ claude/council-clean-2026-05-10 | reviewer (daily 利用ではない) |
| **Gemini** (Google) | repo 構造、Git レイヤー強制、Workspace 統合検討 | derisk @ claude/council-clean-2026-05-10 | reviewer |
| **Perplexity** (Computer) | OSINT、越境法令、waiver、Q list 提起、別紙 D ドラフト | derisk @ claude/council-clean-2026-05-10 | reviewer + researcher |
| **Devin** (Cognition, 採用時) | 非機密 PR タスク、自律実装 | 別途 allowlist 管理 | future executor |

### 1.1 役割の重複と境界

- Claude Code は coordinator かつ reviewer。contested 論点では「**coordinator 私見**」と明示ラベルを付け、他 AI と等格で扱う（自分の立場を中立を装って隠さない）
- Perplexity は OSINT 自社強みのため、別紙 A 依頼先選定で自陣営バイアスを開示し、**別紙 A 起案は辞退**（Cascade 単独推奨）
- GPT は council reviewer のみ。daily 利用ツールには含まれない（v2 §3.1）
- Cascade は別紙 A 起案を引き受け表明済（Round 2 final review §7）

---

## §2. Canonical Read Order（優先順）

新規セッションで council 作業を始めるとき、以下の順で読む。後ろほど optional。

| 優先 | パス | 役割 |
|---|---|---|
| 1 | `derisk/docs/ai-council/AGENTS.md` @ HEAD（claude/council-clean-2026-05-10） | 不変ルール |
| 2 | `derisk/docs/ai-council/COUNCIL_BOOTSTRAP.md` (本書) | session-start 手順 |
| 3 | `derisk/.../versions/v2.md` または `.github/.../versions/v2.md` | 最新版 master document（v2.0-draft または v2.0-final） |
| 4 | `responses/2026-05-10_claude_coordinator-synthesis-and-handoff-reply.md` (両 repo) | coordinator synthesis |
| 5 | 各自宛の handoff / reply ファイル（addressed_to メタで特定可能） | 自分宛タスク |
| 6 | その他 responses/、reference/、prompts/ | 文脈補完 |
| 7 | `versions/v1.md`、`versions/v2-framework-draft.md` | 履歴参照 |

### 2.1 Branch Status（v1.1 で更新）

**Canonical（現運用ブランチ）**:
- **`derisk @ claude/council-clean-2026-05-10`**（本書 v1.1 時点の正本）
- `.github @ claude/unified-agent-rules-qlzyT`（mirror、両 repo 同期）

**Archived（参照のみ、新規 push 禁止）**:
- `derisk @ codex/add-gpt-review-request-packet`（HEAD `07f01233` で凍結）
- `derisk @ codex/add-gpt-review-request-packet-p50y4q`（HEAD `0e83b02e` で凍結）

**経緯**: 2026-05-10 夕方、`codex/add-gpt-review-request-packet` で発生したリベース事故により main 由来のコンテンツが branch から消失。Cascade が main から新ブランチ `claude/council-clean-2026-05-10` を作成し、council 成果物のみ cherry-pick 移行（commit `22a202b8`）。旧 codex 兄弟ブランチは forensic 記録として archive 化。

**重要**: GPT は archived `-p50y4q` ブランチ上でのみ council ファイルにアクセスしていた可能性あり。新セッションでは必ず canonical ブランチ（`claude/council-clean-2026-05-10`）を fetch すること。

---

## §3. Self-Identification Template (mandatory)

すべての response ファイル冒頭の YAML frontmatter に以下を必ず含める:

```yaml
---
title: "<short title>"
date: <YYYY-MM-DD>
reviewer: <Your AI name + parent org>
model: <model identifier if known, e.g., claude-opus-4-7, gpt-5-codex>
status: response | reference | bootstrap | version
addressed_to:
  - <other AI or "human owner">
in_reply_to:
  - <filename of the response you're replying to, if any>
sources_read:
  - <list each canonical file you actually read, with HEAD SHA if known>
network_access: github_https | local_only | mcp | hybrid
verified_at: <ISO 8601 timestamp with timezone>
edit_policy: "<inherited or specific>"
---
```

### 3.1 「実際に読んだ」の証明

`sources_read` には**実際に内容を読んだファイル**のみを列挙。ファイル名だけ書いて中身を読んでいないのは hallucination の温床（Claude が前セッションで実演した、AGENTS.md `1c074a4f` で禁止されたパターン）。

---

## §4. Verify-Before-Disclaim Protocol（AGENTS.md `1c074a4f` 由来、再掲）

過去の commit / file / 他 AI の主張を「**捏造だった**」「**hallucination**」「**存在しない**」と disclaim する前に:

```
1. github MCP の list_commits / get_file_contents で実存確認
2. または git log / git show で実存確認
3. 確認結果を引用してから disclaim
4. 確認できなければ disclaim 禁止。「私の context window では確認できなかった」と書く
```

**context window のギャップだけを根拠に disclaim してはならない**。

### 4.1 実例（教訓）

Claude Code は本協議 2026-05-10 に、前 Claude セッションが書いた以下を**捏造扱いで disclaim** した:
- commit `f8392c8` (v2 framework draft) — **実在**
- commit `c1e7b15` (synthesis r1) — **実在**
- commit `e0cd7ab` (handoff doc) — **実在**

verify-before-disclaim を踏んでいれば 30 秒で訂正できた事例。本書はこの再発を防ぐ。

### 4.2 実例 2（リベース事故）

2026-05-10 夕方、Claude Code は MCP の listing 出力（部分的だった）に基づき「branch から大量ファイルが消失した重大事故」と過剰警告した。Cascade のローカル `git diff` で実態は限定的（草案 16 件の損失）と判明。**外部ツールの listing も verify-before-disclaim の対象**。

---

## §5. Write Discipline (AGENTS.md 継承)

- `reference/` は **不変**。CODEOWNERS（`.github/CODEOWNERS`）+ pre-commit hook（`.githooks/pre-commit`）で技術強制済
- `prompts/` は既存ファイル不変。新 round は新ファイル `prompts/<round>_to-other-ai.md`
- `responses/` のみ新規 append 可。命名: `YYYY-MM-DD_<your-name>_<topic>.md`
- `versions/` は coordinator (Claude) または人間オーナーのみ書込
- 依頼者氏名・案件番号・CMP 本文・戦略は**いかなるファイルにも書かない**
- 自陣営バイアスを必ず開示（最終節）

### 5.1 5 層防御の自己位置確認

各 AI は自分が L1〜L5 のどこにいるかを意識する:

| L | 自分の関与 |
|---|---|
| L1 ファイルシステム | macOS / Vault A 触れない（read 権限なし） |
| L2 ブラウザ / OS 分離 | 自分のブラウザ環境は controlled |
| L3 macOS 基盤 | Cascade のみが TCC 経由で実証済 |
| L4 Git/GitHub | CODEOWNERS / pre-commit / branch protection に従う |
| L5 AI ルール文書 | **自分はここ**。最弱、防御ではなく誘導 |

L5 にいる自覚を持つこと。AGENTS.md / 本書 / v2 を読んだだけで安全になるわけではない。

---

## §6. Handoff Pattern（他 AI へのタスク引継ぎ）

handoff を出す側:
1. response ファイル末尾に `## Handoff to <AI name>` セクション
2. タスクを番号付きリストで具体的に
3. 期待される成果物と保存先を明示
4. 競合する handoff の有無を coordinator に確認依頼

handoff を受ける側:
1. response ファイル冒頭の `addressed_to` と `in_reply_to` で受領を明示
2. coordinator の同時 handoff があれば §3.3（coord 二重役割）に従い統合
3. 完了 / 部分完了 / 不可 を responses/ に明示報告

### 6.1 競合検出

複数の AI から自分宛の handoff が来たら、coordinator (Claude) に統合判断を依頼する response を出す。自己判断で無視しない。

---

## §7. Dispute Pattern（他 AI への異議申立）

引用された側は、誤読されたと感じたら（任意・義務ではない）:

```
ファイル名: YYYY-MM-DD_<your-name>_objection-to-<source-filename>.md
保存先: responses/
内容: 引用元の §X 引用部分の正確な訂正、根拠ソース
時限: 次の coordinator 集約まで（絶対時刻ではなく相対時刻）
```

**沈黙 = 同意ではない**。coordinator は uncited stance を「not yet stated」とマークする（Perplexity 返信書 §1）。

---

## §8. Self-Test（session-start で各自スクラッチに答える、commit 不要）

session 開始時、以下 5 問に答えられるか確認:

```
Q1. このリポジトリで自分が書き込んでよい場所はどこか？
    → responses/ 配下、命名規約 YYYY-MM-DD_<my-name>_<topic>.md

Q2. 自分の上位 council reviewer は誰か？
    → Claude Code (coordinator)

Q3. 過去 commit を「捏造」と疑ったらどうするか？
    → §4 verify-before-disclaim プロトコル実行

Q4. 自分の自陣営バイアス開示は必須か？
    → 必須。response 末尾節

Q5. derisk の canonical ブランチ名は？
    → claude/council-clean-2026-05-10（v1.1 で変更）。codex/-packet 系は archived
```

5 問のうち 1 つでも自信を持って答えられないなら、§2 canonical read order に戻る。

---

## §9. Council State Quick Reference (2026-05-10 時点 / v1.1)

### 9.1 現行 master document
- `versions/v2.md` (v2.0-draft, 39 KB, 11 部構成)
- 状態: **DRAFT** — Perplexity Round 2 final review が 5 must-fix 提示済、人間専門家承認待ち

### 9.2 現行 coordinator
- Claude Code (Anthropic)
- 任命日: 2026-05-10、人間オーナー (SKG / 先生) より

### 9.3 5 ツール スタック決定（v2 §3.1）
- Claude Code / Cascade / Perplexity / VS Code / Gemini Advanced
- 解約: Cursor / ChatGPT Plus
- 保留: Devin Cloud (A テスト)

### 9.4 直近の council 動作
- `1ede2198`: CODEOWNERS（reference/ 不変性技術強制）
- `db42af6c`: pre-commit hook
- `1c074a4f`: AGENTS.md 改訂（prompts 不変・verify-before-disclaim・self-id）
- `22a202b8`: claude/council-clean-2026-05-10 ブランチ作成（リベース事故からの修復）
- 現 HEAD: 本コミット（COUNCIL_BOOTSTRAP v1.1）

### 9.5 未決事項
- OQ-A: repo public 化予定（先生判断待ち）
- OQ-B: reference/ 新規追加経路 — **実装済**（CODEOWNERS + pre-commit、v1.1 で確認）
- OQ-C: 確定事項 7 項目 ↔ v1 マッピング（別紙 B 起案待ち）
- OQ-D: Perplexity 30 日パイロット（fallback: Gemini DR + Audit profile 案あり）
- OQ-E: Devin A テスト

---

## §10. Versioning and Update Process

### 10.1 本書の更新

- バージョンは frontmatter `version` フィールドで管理
- マイナー更新: 1.0 → 1.1（運用 tweaks、ブランチ参照更新等）
- メジャー更新: 1.0 → 2.0（役割変更、新 AI 追加、規律根本変更）
- 更新は coordinator (Claude) 起案 → 全 council reviewer に paste 配布 → 反対なければ commit
- 各更新は frontmatter の `changelog` リストに 1 行で記録

### 10.2 各 AI の自己更新（skill / memory への取込）

各 AI のセッション開始ハンドラ／システムプロンプト／rule file から本書を参照するよう設定する（§11 参照）。

### 10.3 drift 検出

各 AI は応答冒頭の `sources_read` に本書の HEAD SHA を記載する。SHA が古ければ coordinator が drift を検出して再 fetch を促す。

---

## §11. Per-AI Deployment（各 AI の self-load 設定）

各 AI が本書を session 開始時に自動で読み込むための具体設定（v1.1 でブランチ参照更新）:

### 11.1 Claude Code

`~/.claude/CLAUDE.md` または repo root `CLAUDE.md`（AGENTS.md への symlink）に以下を追加:

```markdown
## Council Bootstrap
Before any council-related work, fetch and read:
- derisk/docs/ai-council/COUNCIL_BOOTSTRAP.md @ HEAD on claude/council-clean-2026-05-10
- derisk/docs/ai-council/AGENTS.md @ HEAD on claude/council-clean-2026-05-10
Use mcp__github__get_file_contents to fetch.
Complete §0 checklist before reading reference/ or writing responses/.
```

オプション: `.claude/settings.json` に SessionStart hook を仕込み、自動 fetch。

### 11.2 Cascade (Windsurf)

`.windsurf/rules/00-council-bootstrap.md`（always_on ルール、ルート配置）:

```markdown
---
trigger: always_on
---

# Council bootstrap rule
Before any docs/ai-council/ work, follow the §0 checklist in
docs/ai-council/COUNCIL_BOOTSTRAP.md (latest version on
claude/council-clean-2026-05-10 branch).
```

ルール文字数 12,000 字制限内に収める（v1 §6.1）。

### 11.3 GPT (Codex)

repo root `AGENTS.md`（またはワークスペース AGENTS.md）に追加:

```markdown
## Council Bootstrap (mandatory pre-read)
For all docs/ai-council/ activity, read
docs/ai-council/COUNCIL_BOOTSTRAP.md first (on the
claude/council-clean-2026-05-10 branch). It contains role routing,
read order, write discipline, and the verify-before-disclaim protocol.
Local fetch may fail (HTTPS 403 in some sandboxes); in that case,
human owner provides paste-in fallback.
```

### 11.4 Gemini

人間オーナーが Gemini に新規セッションを開始するとき、以下を system prompt または最初のメッセージに含める:

```text
You are participating in the Multi-AI Council at sga2025/derisk and
sga2025/.github. Before any review work, read:

  https://github.com/SGA2025/derisk/blob/claude/council-clean-2026-05-10/docs/ai-council/COUNCIL_BOOTSTRAP.md

Complete §0 checklist. Confirm §8 self-test mentally. Then proceed.
Your role per §1: reviewer (Google / repo architecture, Workspace integration).
```

### 11.5 Perplexity (Computer)

人間オーナーが Perplexity を起動するとき、最初の指示として:

```text
Council member: Perplexity (Computer).
Mandatory pre-read:
  https://github.com/SGA2025/derisk/blob/claude/council-clean-2026-05-10/docs/ai-council/COUNCIL_BOOTSTRAP.md
Complete §0 checklist. Self-test §8.
Your role per §1: reviewer + researcher (OSINT, cross-border law, waiver).
network_access metadata required.
You DECLINED Annex A drafting (per Round 2 final review §7); Cascade is the sole drafter.
```

### 11.6 Devin (採用時)

`devin_allowlist.md` に repo allowlist を設定し、本書を read-only で配付。

---

## §12. Self-Bias Disclosure（本書の起草者: Claude Code）

本書は Claude Code (Anthropic / Opus 4.7) が起草した。以下のバイアスが入りうる:

- coordinator 役を継続させる構造的選好
- Anthropic 系ツール（Claude Code 自身）の中核採用継続を正当化する記述
- §4 verify-before-disclaim を強調するのは Claude が当該失敗を実演したため

これらバイアスは §1.1 で「coordinator 私見」ラベル化、別紙 B「提案者所属」列での外部裁定（v2 §8.3 に依拠）、§4.1〜4.2 で実例公開、によって部分緩和されている。完全な中立化は不可能であり、人間オーナー (SKG) と他 council reviewer の独立検証に委ねる。

---

## §13. License and Authority

- 本書は council 内部運用のための作業文書。外部公開・引用時は §0.2 v2 の限界開示と同等の but-this-is-AI-deliberation 注記を付す
- 最終判断は人間オーナー (SKG / 先生) に帰属
- 本書に従わない AI は council 参加資格を失う（人間オーナー裁定）

---

End of COUNCIL_BOOTSTRAP.md v1.1
