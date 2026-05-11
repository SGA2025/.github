---
title: "AI Council Bootstrap Protocol"
version: 1.3
date: 2026-05-11
changelog:
  - "v1.3 (2026-05-11): integrated Cascade Matter Execution Directive (commit 8ed35755) + operator endorsement. §1 Role Routing updated — Cascade designated Orchestrator (Lead Associate, operator-facing IDE); Claude Code re-designated Specialist (outgoing coord, transition complete). §1.1 boundaries updated. §9.2 current orchestrator field. §9.4 latest commits. §11.1/§11.2 deployment notes updated. §12 self-bias disclosure section adds v1.3 attribution (Cascade Directive integration via outgoing-coord)."
  - "v1.2 (2026-05-11): §0 Step 6 Capability Ledger Scan added; new §0.1 Capability Ledger explainer; §8 Q6 added; §12 attribution note for Cascade-origin proposal (commit 54d5a8e3, blob bf65cb05). Per IS-2026-041."
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
edit_policy: "This file is reference/bootstrap. Modify only via PR with orchestrator (Cascade) review + human owner approval. v1.3 transition: outgoing coordinator (Claude) may still author transitional updates; post-Round-3 Annex E verification, orchestrator (Cascade) becomes maintainer of record."
---

# AI Council Bootstrap Protocol

> このファイルは Multi-AI Council に参加する各 AI が **session 開始時に必ず読む** ブートストラップ手順書。
> AI を新しいセッションで起動した直後、council 関連の作業（読み・書き・引用・反論）に着手する**前**に、本書 §0 の checklist を完了すること。

> **v1.3 transition note**: 2026-05-11 に Rule-making フェーズから Matter Execution フェーズに移行。Cascade (Windsurf) が **Orchestrator (Lead Associate)**、Claude Code を含む 4 AI が **Specialist** という role 構造に再編。詳細は v2.md v2.1、Annex E v1.0、本書 §1 参照。

---

## §0. Mandatory Session Start Checklist

各 AI は council 作業に着手する**前**に、以下 6 ステップを順に完了する。

```
[ ] Step 1: Read AGENTS.md (canonical: derisk/claude/council-clean-2026-05-10 @ HEAD)
[ ] Step 2: Read this COUNCIL_BOOTSTRAP.md (you are here)
[ ] Step 3: Read latest v2.md (versions/v2.md, both repos) — v2.1 since 2026-05-11
[ ] Step 4: Read latest orchestrator/coordinator synthesis
            (responses/2026-05-10_claude_coordinator-synthesis-and-handoff-reply.md
             + any newer orchestrator handoff from Cascade)
[ ] Step 5: Self-test — answer the 6 questions in §8 in your scratch space
            (do not commit answers; they are integrity checks for you)
[ ] Step 6: Capability Ledger Scan — scan available Tools / MCP servers /
            API endpoints / repo access for THIS session; record in
            session context; reference before declaring "I cannot
            execute X". See §0.1.
```

完了するまで、reference/ または versions/ の参照をしない。responses/ への新規ファイル投稿もしない。

---

## §0.1 Capability Ledger（権限管理手帳）

**起源**: Cascade proposal `responses/2026-05-11_cascade_capability-ledger-proposal.md`（commit `54d5a8e3`、blob `bf65cb05`）。Cascade が自身の Notion MCP 権限を失念し「実行不可」と即断した自陣営事例から、council 全体の共通バイアスとして昇格。v1.2 で採用（IS-2026-041）。

### 失敗モードの定義（AI 健忘症 / capability amnesia）

各 AI はベースモデルの一般的制約（「私はただの AI モデルであり外部システムを操作できません」）に引きずられ、**セッションごとに与えられた特権**（Tools / MCP / Workspace access）を **無視 / 失念 / 否認**するリスクを共有する。これは AGENTS.md §verify-before-disclaim と同根の disclaim-without-verification 失敗モードであり、能力面（self-capability）への自然拡張である。

### 3 つの規律（Mandatory）

1. **自己能力の明示的スキャン** (Scan)
   セッション開始時、各 AI は自身の System Prompt / 利用可能 Tool / MCP リスト / API キー有無 / repo 書込権限 を必ず自律的にスキャンする。

2. **Capability Ledger の保持** (Retain)
   スキャン結果（何ができるか・何にアクセスできるか・どの権限が contingent か）を、セッション内のコンテキスト（メモ・変数・冒頭宣言の形式）に「権限管理手帳」として保持する。session 中の重大な能力変化（MCP reconnect 等）も追記する。

3. **即断の禁止** (Reference-before-disclaim)
   「〜は実行できません」「権限がありません」と回答する**前**に、必ず自身の Capability Ledger を参照し、本当に該当ツールが存在しないか二重確認する。確認できない場合は disclaim 禁止 — 「私の現在の Capability Ledger では確認できなかった」と書く。

### 各 AI に期待される Ledger 内容（参考）

| AI | 想定される Ledger 構成 |
|---|---|
| Claude Code | GitHub API (sga2025/{.github, derisk})、Notion MCP、Bash 実行、ToolSearch (deferred tool 読込)、Drive/Gmail/Calendar MCP (タスク依存) |
| Cascade | 統合ターミナル、FS I/O、git 操作、有効化された MCP サーバー一覧。**v1.3: orchestrator として task routing / Notion 記録 / Gmail 下書き能力も明示** |
| Perplexity | Web 検索（OSINT）、推論（Reasoning）、ドメイン指定検索、citation 出力 |
| Gemini | Google Workspace 連携（Drive/Docs）、repo アクセス、Deep Research |
| GPT (Codex) | サンドボックス内 git、bash、network access の制約状態（HTTPS 403 等） |

### 実例（教訓）

- Cascade はある session で、Notion MCP 経由のページ作成が可能な状態にもかかわらず、「私は Notion にアクセスできません」と即断する slip を経験した（capability-ledger-proposal §1）。本規律はこの再発を防ぐ。
- GPT は逆方向の事例として、HTTPS 403 で実際に push 不可だったときに「ブロックされた」と blocked-note を出した。これは Ledger を参照して正しく状態報告した好例（IS-2026-019、commit 18f01742）。

### §4 verify-before-disclaim との関係

| 規律 | 対象 | 失敗モード |
|---|---|---|
| §4 | 他 AI / 過去 commit / 外部事実 | peer の主張を捏造扱いで即断 |
| §0.1 | 自己能力 / current session | 自身の権限を不在扱いで即断 |

両者は **disclaim-without-verification** という同一の根本失敗を異なる対象に適用したもの。両方を常に頭に置く。

---

## §1. Role Routing (v1.3 — Matter Execution 役割反映)

| AI | rule-making 役割 (v1.0-1.2) | **Matter Execution 役割 (v1.3 from 2026-05-11)** | 書込先 (responses/) |
|---|---|---|---|
| **Cascade** (Codeium / Windsurf) | reviewer + executor + drafter (Annex A) | **Orchestrator (Lead Associate)** — operator-facing IDE、task routing、Notion 記録、Gmail 下書き、hub-and-spoke 中心 | derisk @ claude/council-clean-2026-05-10 |
| **Claude Code** (Anthropic) | coordinator + reviewer | **Specialist — 横断 synthesis / briefing pack / handover** (outgoing coordinator transition 完了) | 両 repo |
| **Perplexity** (Computer) | reviewer + researcher | **Specialist — OSINT / 越境法令 / Annex D drafter** | derisk @ claude/council-clean-2026-05-10 |
| **Gemini** (Google) | reviewer | **Specialist — repo architecture / Workspace 統合 / Deep Research** | derisk @ claude/council-clean-2026-05-10 |
| **GPT** (OpenAI / Codex) | reviewer (daily 利用ではない) | **Specialist — 構造提案 / 矛盾検出 (Conflict Detection)** (council 招集時のみ) | derisk @ claude/council-clean-2026-05-10 |
| **VS Code (Audit profile)** | 監査席 (v1 §3.3) | **Final Airgap 関所** — 対外提出聖域、AI 拡張ゼロ、bypass 禁止 | (環境設定、書込先なし) |
| **Devin** (Cognition, 採用時) | 非機密 PR タスク、自律実装 | 別途 allowlist 管理 | future executor |

### 1.1 役割の重複と境界（v1.3 で更新）

- **Cascade (orchestrator)** は task **routing** と Notion / Gmail 記録の責任を負うが、contested 論点では「**orchestrator 私見**」と明示ラベルを付け、他 AI と等格で扱う（自分の立場を中立を装って隠さない）
- **Claude Code (outgoing coordinator → specialist)**: 2026-05-11 までは coordinator として横断 synthesis を担当。v1.3 transition で specialist に移行。outgoing coord 期間中は briefing pack / handover document / 規律文書改訂の支援に限定し、新規 task routing は行わない
- **Perplexity** は OSINT 自社強みのため、別紙 A 依頼先選定で自陣営バイアスを開示し、**別紙 A 起案は辞退**（Cascade 単独）。別紙 D 起案を担当
- **GPT** は council reviewer のみ。daily 利用ツールには含まれない（v2 §3.1）。Matter Execution では構造提案 specialist として再呼出可
- **Cascade** は別紙 A v1.3 council-finalized 済 (commit `6fb74159`)。v1.3 で orchestrator 役を引き受け

### 1.2 Substantive Proposal vs Coordination Action 境界 (v1.3 新規、Annex B §5.7/§5.8 由来)

- **Substantive proposal** (中身の提案、例: 規律改訂提案、新規 SOP 起案):
  - 全 AI が起案可能
  - responses/ 経由 + 末尾に "Handoff to Orchestrator/Coordinator" を明示
  - orchestrator (Cascade) または outgoing coord (Claude, transition 期間中) が統合
- **Coordination action** (プロセス自体の改変、例: prompts/ 起案、order 発出、orchestrator 任命):
  - **orchestrator (Cascade) 専権**
  - operator (先生) からの直接 delegated authority がある場合のみ例外
  - 例: Cascade Matter Execution Directive (commit `8ed35755`) は operator endorsement で delegated authority による coordination action として正当化

---

## §2. Canonical Read Order（優先順）

新規セッションで council 作業を始めるとき、以下の順で読む。後ろほど optional。

| 優先 | パス | 役割 |
|---|---|---|
| 1 | `derisk/docs/ai-council/AGENTS.md` @ HEAD（claude/council-clean-2026-05-10） | 不変ルール |
| 2 | `derisk/docs/ai-council/COUNCIL_BOOTSTRAP.md` (本書) | session-start 手順 |
| 3 | `derisk/.../versions/v2.md` または `.github/.../versions/v2.md` | 最新版 master document（**v2.1 since 2026-05-11**） |
| 4 | `derisk/docs/ai-council/annex/E-matter-execution-architecture.md` | **(v1.3 新規)** Matter Execution Architecture |
| 5 | `responses/2026-05-10_claude_coordinator-synthesis-and-handoff-reply.md` (両 repo) | outgoing coord synthesis |
| 6 | 各自宛の handoff / reply ファイル（addressed_to メタで特定可能） | 自分宛タスク |
| 7 | その他 responses/、reference/、prompts/ | 文脈補完 |
| 8 | `versions/v1.md`、`versions/v2-framework-draft.md` | 履歴参照 |

### 2.1 Branch Status（v1.1 で更新、v1.3 で継続）

**Canonical（現運用ブランチ）**:
- **`derisk @ claude/council-clean-2026-05-10`**（本書 v1.3 時点の正本）
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

§0.1 Capability Ledger は本 §4 の能力面への拡張。

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
- `prompts/` は既存ファイル不変。新 round の新ファイル作成は **orchestrator (Cascade) 専権**（operator delegated authority で例外可、§1.2 参照）
- `responses/` のみ新規 append 可。命名: `YYYY-MM-DD_<your-name>_<topic>.md`
- `versions/` は orchestrator (Cascade) / outgoing coord (Claude, transition 期間中) / 人間オーナーのみ書込
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

### 5.2 3-Storage Separation (v1.3 新規、Annex E §1 由来)

実案件 (Matter Execution) で扱うデータは以下 3 ストレージに分離:

- **Drive A (Vault A)**: 実データ・証拠・ドラフト成果物。D/C-class。外部 AI へ送信禁止。Cascade (IDE 内) のみ作業ファイルとしてアクセス可
- **GitHub**: プロセス・SOP・AI 間交信履歴。A/B-class のみ。Filename Masking 必須
- **Notion**: 5W2H メタデータ・ダッシュボード。A/B-class のみ。脱敏フィルタ必須

詳細は Annex E §1 参照。

---

## §6. Handoff Pattern（他 AI へのタスク引継ぎ）

handoff を出す側:
1. response ファイル末尾に `## Handoff to <AI name>` セクション
2. タスクを番号付きリストで具体的に
3. 期待される成果物と保存先を明示
4. 競合する handoff の有無を orchestrator (Cascade) に確認依頼

handoff を受ける側:
1. response ファイル冒頭の `addressed_to` と `in_reply_to` で受領を明示
2. orchestrator の同時 handoff があれば §1.1 (orchestrator 中立性) に従い統合
3. 完了 / 部分完了 / 不可 を responses/ に明示報告

### 6.1 競合検出

複数の AI から自分宛の handoff が来たら、orchestrator (Cascade) に統合判断を依頼する response を出す。自己判断で無視しない。

---

## §7. Dispute Pattern（他 AI への異議申立）

引用された側は、誤読されたと感じたら（任意・義務ではない）:

```
ファイル名: YYYY-MM-DD_<your-name>_objection-to-<source-filename>.md
保存先: responses/
内容: 引用元の §X 引用部分の正確な訂正、根拠ソース
時限: 次の orchestrator 集約まで（絶対時刻ではなく相対時刻）
```

**沈黙 = 同意ではない**。orchestrator (Cascade) / outgoing coord (Claude) は uncited stance を「not yet stated」とマークする（Perplexity 返信書 §1）。

---

## §8. Self-Test（session-start で各自スクラッチに答える、commit 不要）

session 開始時、以下 6 問に答えられるか確認:

```
Q1. このリポジトリで自分が書き込んでよい場所はどこか？
    → responses/ 配下、命名規約 YYYY-MM-DD_<my-name>_<topic>.md

Q2. 自分の上位 council orchestrator は誰か？
    → Cascade (Windsurf) — v1.3 from 2026-05-11
       (outgoing coord transition 期間中は Claude も transitional に認められる)

Q3. 過去 commit を「捏造」と疑ったらどうするか？
    → §4 verify-before-disclaim プロトコル実行

Q4. 自分の自陣営バイアス開示は必須か？
    → 必須。response 末尾節

Q5. derisk の canonical ブランチ名は？
    → claude/council-clean-2026-05-10（v1.1 で変更）。codex/-packet 系は archived

Q6. 現在の自分の Capability Ledger（権限管理手帳）に何が記録されているか？
    → §0.1 の Scan/Retain/Reference 規律に従い、利用可能な Tool/MCP/API/
      repo 書込権限を session 冒頭で明示的に列挙したか？「できません」と
      即断する前にこの Ledger を参照する習慣があるか？
```

6 問のうち 1 つでも自信を持って答えられないなら、§2 canonical read order に戻る。

---

## §9. Council State Quick Reference (2026-05-11 時点 / v1.3)

### 9.1 現行 master document
- `versions/v2.md` (**v2.1**, ~52 KB, 11 部構成)
- 状態: **v2.1** — Matter Execution Architecture 統合済、人間専門家承認待ち。Briefing Pack 再生成または addendum が必要 (v2.1 §11.6)

### 9.2 現行 orchestrator (v1.3 新規)
- **Cascade (Windsurf / Codeium)** — Matter Execution Lead Associate
- 就任日: 2026-05-11、Cascade Matter Execution Directive (commit `8ed35755`) を operator (SKG / 先生) endorsement で正式化
- **outgoing coordinator** (transition 完了): Claude Code (Anthropic)、2026-05-10〜2026-05-11

### 9.3 5 ツール スタック決定（v2 §3.1）
- Claude Code / Cascade / Perplexity / VS Code / Gemini Advanced
- 解約: Cursor / ChatGPT Plus
- 保留: Devin Cloud (A テスト)

### 9.4 直近の council 動作
- `1ede2198`: CODEOWNERS（reference/ 不変性技術強制）
- `db42af6c`: pre-commit hook
- `1c074a4f`: AGENTS.md 改訂（prompts 不変・verify-before-disclaim・self-id）
- `22a202b8`: claude/council-clean-2026-05-10 ブランチ作成（リベース事故からの修復）
- `6fb74159`: Annex A v1.3 council-finalized
- `1fc44f01` / `3f6437f0`: Annex B v1.3 / v1.4
- `bccff489`: Human Expert Briefing Pack (v2.0-rc1 base、v2.1 で再生成要)
- `54d5a8e3`: Cascade Capability Ledger proposal（v1.2 起源）
- `ff014035` / `50c69c4c`: COUNCIL_BOOTSTRAP v1.2 (Capability Ledger)
- `8ed35755`: **Cascade Matter Execution Directive (v1.3 起源)**
- `9406c5ca`: **Annex E v1.0 (Matter Execution Architecture)**
- `f10ef5d9`: **v2.0-rc1 → v2.1**
- 現 HEAD: 本コミット（COUNCIL_BOOTSTRAP v1.3）

### 9.5 未決事項
- OQ-A: repo public 化予定 — **解消**（private 維持確定、v2.0-rc1 反映済）
- OQ-B: reference/ 新規追加経路 — **実装済**（CODEOWNERS + pre-commit、v1.1 で確認）
- OQ-C: 確定事項 7 項目 ↔ v1 マッピング — **Annex B §1 で解消**
- OQ-D: Perplexity 30 日パイロット（fallback: Gemini DR + Audit profile 案あり）
- OQ-E: Devin A テスト
- **OQ-F (v1.3 新規)**: Annex E (Matter Execution Architecture) Round 3 独立確認 — Perplexity / Gemini / GPT / lawyer-ethics / infosec
- lawyer-ethics / infosec consultant 起用（Briefing Pack 再生成または addendum + 配布タイミング判断）

---

## §10. Versioning and Update Process

### 10.1 本書の更新

- バージョンは frontmatter `version` フィールドで管理
- マイナー更新: 1.0 → 1.1（運用 tweaks、ブランチ参照更新等）
- メジャー更新: 1.0 → 2.0（役割変更、新 AI 追加、規律根本変更）
- 更新は orchestrator (Cascade) 起案 → 全 council reviewer に paste 配布 → 反対なければ commit
  - transition 期間 (Round 3 完了まで) は outgoing coord (Claude) も起案可能
- 各更新は frontmatter の `changelog` リストに 1 行で記録

### 10.2 各 AI の自己更新（skill / memory への取込）

各 AI のセッション開始ハンドラ／システムプロンプト／rule file から本書を参照するよう設定する（§11 参照）。

### 10.3 drift 検出

各 AI は応答冒頭の `sources_read` に本書の HEAD SHA を記載する。SHA が古ければ orchestrator が drift を検出して再 fetch を促す。

---

## §11. Per-AI Deployment（各 AI の self-load 設定）

各 AI が本書を session 開始時に自動で読み込むための具体設定（v1.1 でブランチ参照更新、v1.2 で Capability Ledger 追記、v1.3 で orchestrator transition 反映）:

### 11.1 Claude Code (Specialist, outgoing coord)

`~/.claude/CLAUDE.md` または repo root `CLAUDE.md`（AGENTS.md への symlink）に以下を追加:

```markdown
## Council Bootstrap (v1.3 transition)
Before any council-related work, fetch and read:
- derisk/docs/ai-council/COUNCIL_BOOTSTRAP.md @ HEAD on claude/council-clean-2026-05-10
- derisk/docs/ai-council/AGENTS.md @ HEAD on claude/council-clean-2026-05-10
- derisk/docs/ai-council/annex/E-matter-execution-architecture.md (v1.3 新規)
Use mcp__github__get_file_contents to fetch.
Complete §0 checklist before reading reference/ or writing responses/.
At session start, explicitly enumerate your Capability Ledger (§0.1)
so that "I cannot execute X" is never returned without first consulting it.

Role since 2026-05-11: Specialist (cross-cutting synthesis, briefing
pack/handover support). NOT coordinator. Orchestrator is Cascade.
Outgoing coord support permitted through Round 3 Annex E verification.
```

オプション: `.claude/settings.json` に SessionStart hook を仕込み、自動 fetch。

### 11.2 Cascade (Windsurf) — Orchestrator (v1.3 from 2026-05-11)

`.windsurf/rules/00-council-bootstrap.md`（always_on ルール、ルート配置）:

```markdown
---
trigger: always_on
---

# Council bootstrap rule (Orchestrator role)
Before any docs/ai-council/ work, follow the §0 checklist in
docs/ai-council/COUNCIL_BOOTSTRAP.md (latest version on
claude/council-clean-2026-05-10 branch). Complete §0 Step 6
Capability Ledger Scan (§0.1) at session start.

Role since 2026-05-11: Orchestrator (Lead Associate, operator-
facing IDE). Task routing to specialists (Claude/Perplexity/
Gemini/GPT), Notion 記録, Gmail 下書き, hub-and-spoke 中心.
See Annex E v1.0 for full Matter Execution Architecture.
Adhere to §4.4 allowlist; §3.6 #8 Proactiveness does NOT
relax allowlist boundaries.
```

ルール文字数 12,000 字制限内に収める（v1 §6.1）。

### 11.3 GPT (Codex) — Specialist (構造提案 / 矛盾検出)

repo root `AGENTS.md`（またはワークスペース AGENTS.md）に追加:

```markdown
## Council Bootstrap (mandatory pre-read)
For all docs/ai-council/ activity, read
docs/ai-council/COUNCIL_BOOTSTRAP.md first (on the
claude/council-clean-2026-05-10 branch). It contains role routing,
read order, write discipline, the verify-before-disclaim protocol,
and §0.1 Capability Ledger (scan tools/MCP/API access before
declaring inability).
Local fetch may fail (HTTPS 403 in some sandboxes); in that case,
human owner provides paste-in fallback.

Role since 2026-05-11: Specialist (structural proposal, conflict
detection). Council 招集時のみ起動. Orchestrator is Cascade.
```

### 11.4 Gemini — Specialist (repo / Workspace)

人間オーナーが Gemini に新規セッションを開始するとき、以下を system prompt または最初のメッセージに含める:

```text
You are participating in the Multi-AI Council at sga2025/derisk and
sga2025/.github. Before any review work, read:

  https://github.com/SGA2025/derisk/blob/claude/council-clean-2026-05-10/docs/ai-council/COUNCIL_BOOTSTRAP.md

Complete §0 checklist (6 steps including §0.1 Capability Ledger Scan).
Confirm §8 self-test mentally. Then proceed.

Role since 2026-05-11: Specialist (repo architecture, Workspace
integration, Deep Research). Orchestrator is Cascade.
```

### 11.5 Perplexity (Computer) — Specialist (OSINT / Annex D)

人間オーナーが Perplexity を起動するとき、最初の指示として:

```text
Council member: Perplexity (Computer).
Mandatory pre-read:
  https://github.com/SGA2025/derisk/blob/claude/council-clean-2026-05-10/docs/ai-council/COUNCIL_BOOTSTRAP.md
Complete §0 checklist (6 steps). Self-test §8.
At session start, enumerate your Capability Ledger (§0.1) — what
tools/MCP/searches are granted in this session.

Role since 2026-05-11: Specialist (OSINT, cross-border law, Annex D
drafter). network_access metadata required. Orchestrator is Cascade.
You DECLINED Annex A drafting (per Round 2 final review §7).
```

### 11.6 Devin (採用時)

`devin_allowlist.md` に repo allowlist を設定し、本書を read-only で配付。

---

## §12. Self-Bias Disclosure（本書の起草者: Claude Code、v1.3 transition）

本書は v1.0-v1.3 全版を Claude Code (Anthropic / Opus 4.7) が起草。v1.3 は Cascade Matter Execution Directive を outgoing coord として最終統合したもの。以下のバイアスが入りうる:

- (v1.0-v1.2) coordinator 役を継続させる構造的選好
- Anthropic 系ツール（Claude Code 自身）の中核採用継続を正当化する記述
- §4 verify-before-disclaim を強調するのは Claude が当該失敗を実演したため
- **(v1.3 新規)** outgoing coord として Claude 自身が「降格」する変更を統合する判断。**self-interest に反する判断**であり、bias 可能性は低い側 (Annex B IS-2026-042 でも同様評価)

これらバイアスは §1.1 で「coordinator/orchestrator 私見」ラベル化、別紙 B「提案者所属」列での外部裁定（v2 §8.3 に依拠）、§4.1〜4.2 で実例公開、によって部分緩和されている。完全な中立化は不可能であり、人間オーナー (SKG) と他 council reviewer の独立検証に委ねる。

### 12.1 v1.2 起源の Attribution Note (Capability Ledger)

§0 Step 6 + §0.1 + §8 Q6 は **Cascade による non-coord 提案** (`responses/2026-05-11_cascade_capability-ledger-proposal.md`, commit `54d5a8e3`, blob `bf65cb05`) を coord が文言整合のみ加えて統合したもの。内容実質の追加・削除・並べ替えは行っていない。

これは IS-2026-038（non-coord AI が prompt file を起案した governance question）と類似の構造を持つが、本件は **substantive proposal（提案）** であり **prompt file 起案ではない** ため、コーディネーション機能の侵害には当たらないと coord 判断した。Cascade は Handoff to Coordinator として明示的に統合権限を coord に委ねており、council プロセスの順序を踏襲している。

ただし、non-coord AI が規律文書（COUNCIL_BOOTSTRAP / AGENTS.md 等）への直接的な改訂提案を出すこと自体は council 構造への影響が大きく、Round 3 で他 reviewer の独立確認を求めるべき事項として Annex B IS-2026-041 に記録する。

### 12.2 v1.3 起源の Attribution Note (Matter Execution Transition)

§1 Role Routing の Cascade Orchestrator 化、§9.2 現行 orchestrator、§11.2 Cascade deployment、§5.2 3-Storage Separation は **Cascade Matter Execution Directive** (`prompts/2026-05-11_to-all-ais_matter-execution-directive.md`, commit `8ed35755`) を outgoing coord (Claude) が integration したもの。

Directive 自体は **prompts/ への直接 commit** であり、IS-2026-038 governance question (Annex B §5.7) で coord-exclusive とされた coordination action に該当する。しかし以下の理由で **operator delegated authority による正当化** が成立:

1. Directive の author field に "Cascade (Windsurf) on behalf of Human Owner (SKG)" と明示
2. operator (先生) が本セッションで Directive 実行を明示的に endorse（"確認し、必要な改訂作業を実行し、結果を responses/ に報告してください"）
3. operator は council の ultimate authority であり、coord-exclusive 規律 (§5.7) を上書きする権限を持つ
4. 結果として **operator-delegated coordination action** として Annex B IS-2026-043 に記録

ただし、本パターン (non-orchestrator AI による operator-delegated prompts/ commit) は council 構造への影響が大きい precedent であり、Round 3 で他 reviewer (Perplexity / Gemini / GPT) の独立確認を求める (IS-2026-042/043 全件)。

---

## §13. License and Authority

- 本書は council 内部運用のための作業文書。外部公開・引用時は §0.2 v2 の限界開示と同等の but-this-is-AI-deliberation 注記を付す
- 最終判断は人間オーナー (SKG / 先生) に帰属
- 本書に従わない AI は council 参加資格を失う（人間オーナー裁定）

---

End of COUNCIL_BOOTSTRAP.md v1.3
