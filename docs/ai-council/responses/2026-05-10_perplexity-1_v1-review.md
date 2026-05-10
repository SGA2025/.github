# Response from Perplexity (round 1) on v1 review

**Date**: 2026-05-10
**Prompt source**: ../prompts/2026-05-10_to-other-ai.md (and the v1 master document)
**AI version / model**: Perplexity (Pro tier — model not disclosed by responder)
**Format**: verbatim (Japanese)
**Operator note**: First-round review of the v1 master document. Delivered prior
to Gemini's first response. Resulted in four explicit modification proposals,
the 5-line "補足" passage that the lead AI accepted in full, and the
OSINT 3-tier classification later corroborated by Gemini.

---

## Verbatim Response

結論だけ言うと、この文書の方向性には大筋で賛成です。ただし、最終版にする前に「Claude Code＝安全なローカル処理」と読める箇所、Devin評価方法、ChatGPT/Gemini解約判断、VS Code監査席の実装方法を少し修正した方がよいです。

## 総合判定

私の評価は「85点、実務投入前に4点修正」です。
特に優れているのは、AIツール選定を「便利さ」ではなく、守秘・データ流出・監査・回復可能性の観点から設計している点です。

ただし、最も重要な修正点は、**Claude Code** を「ローカルで動くから機密処理に安全」と表現しすぎないことです。Claude Codeはローカルファイルシステム、Git、コマンド実行、CLAUDE.md、MCPと相性がよい一方、LLMサービス利用である以上、機密本文を読ませる設計には慎重さが必要です。
日本の弁護士業務では、生成AIプロンプトに秘密保持義務の対象情報をそのまま入力すること自体が問題になり得ると指摘されており、個人情報を含むプロンプト送信は第三者提供や守秘義務違反の論点を生み得ます。

## 修正すべき点

### 1. Claude Codeの位置づけ

文書では「機密案件はClaude Code（ローカル処理）のみ」とありますが、ここは「Claude Codeを使う場合でも、機密本文をモデルに送らない設計に限定」と書き換えるべきです。
より正確には、「Claude Codeはローカル作業を制御する実行環境であり、完全なオンプレミスLLMではない。よって、機密原本は隔離し、metadata inventory、ファイル名、ハッシュ、分類ラベル、人工承認済み抜粋だけを扱う」とするのが安全です。

推奨修正文：

> 機密案件にClaude Codeを使う場合でも、原則として機密本文そのものをLLMに読ませない。Claude Codeはsandbox内で、metadata inventory、スクリプト生成、ルール検証、Git差分確認などに限定し、本文処理が必要な場合は別途の承認ゲートを設ける。

これは先生のCMP境界ルール、すなわちD-class禁止、Phase 2では本文抽出・OCR・脱敏・原件コピー・Vault A書込み禁止という既存設計とも整合します。

### 2. Devin評価方法

「1か月不使用テスト」は少し粗いです。
不使用テストだと、Devinの価値を測るというより「使わない習慣に慣れる」だけになる可能性があります。

よりよいのは、4週間の「タスク別A/Bテスト」です。
DevinはWindsurf 2.0に統合され、Devin利用分がWindsurfの共有quotaから消費される構造なので、価値評価は「使ったか/使わなかったか」ではなく、「quota消費に見合う成果が出たか」で見るべきです。

推奨評価表：

| 週 | タスク | Devin使用 | 比較対象 | 判定指標 |
|---|---|---|---|---|
| Week 1 | GitHub Pages軽微修正 | 使う | Windsurf | PR品質、修正回数、所要時間 |
| Week 2 | データ可視化MVP | 使う | Claude Code | 成果物の完成度、レビュー負荷 |
| Week 3 | OSINT公開資料整理 | 使わない | Gemini/Claude | Devinがなくて困ったか |
| Week 4 | サイト保守/依存関係更新 | 使う | 手作業 | 安全性、差分の理解容易性 |

判定基準は、「月3件以上、Devinでなければ面倒なPR型タスクがあるか」です。
これを満たすならWindsurf Max継続、満たさないならPro降格が合理的です。

### 3. VS Code監査席

VS Code Audit profileを作る方針は正しいですが、「VS Codeでよいか、VSCodiumにすべきか」は一度検討してよいです。
VS Codeは公式設定で `telemetry.telemetryLevel = "off"` にすればVS Code本体のテレメトリを停止できる一方、拡張機能は独自にデータ収集する場合があり、VS Code本体の設定では完全に制御されません。

したがって、Audit profileは以下のように定義するのがよいです。

```json
{
  "telemetry.telemetryLevel": "off",
  "extensions.ignoreRecommendations": true,
  "github.copilot.enable": {
    "*": false
  }
}
```

さらに厳格にするなら、監査席は「VS Code」ではなく「VSCodiumまたはVS Code拡張ゼロプロファイル」と書く方が安全です。
ただし実務上は、先生がすでにVS Codeをインストール済みなら、まずはVS CodeのAudit profileを作り、AI拡張・Copilot・Claude Code拡張・Windsurf連携を入れない運用で十分です。

### 4. ChatGPT解約判断

ChatGPT Plus解約は、現状の「一般会話・質問のみ」という用途なら妥当です。
ただし、他AIへの諮問や比較検証を重視するなら、ChatGPTを完全解約する前に1か月だけ「多角協議用」として残す選択肢もあります。

ChatGPTに固有の潜在価値は、カスタムGPT、音声、画像・動画系、一般ユーザー向け文章のトーン調整、広いエコシステムです。
しかし、先生の中核用途が法律実務、OSINT、Obsidian、GitHub、Devin/Windsurfである以上、ChatGPT Plusは「必須インフラ」ではなく「比較・表現・一般相談用」です。

私の修正案：

| 判断 | 内容 |
|---|---|
| 即時解約でもよい場合 | 本当に一般質問だけで、Claudeで代替できる場合 |
| 1か月保留すべき場合 | ChatGPTにもこの協議文書を投げ、応答品質を比較したい場合 |
| 継続すべき場合 | Voice、画像、Custom GPT、非法律系の一般用途を毎週使う場合 |

## 個別論点への回答

### Cursor解約

Cursor Pro解約は妥当です。
先生はWindsurfを主に使い、Cursorは直近でほぼ未使用という前提があり、Windsurf MaxとClaude CodeがあるならCursorの常用価値は低いです。

ただし、完全削除ではなく「アカウント維持・課金停止」がよいです。
将来、Windsurf/Devinの出力に違和感があるとき、Cursorは別エンジンによるセカンドオピニオンとして再投入できます。

### Gemini Advanced継続

Gemini Advanced継続は妥当です。
先生がDeep Researchを月3〜5回使っているなら、これはClaudeやChatGPTの一般チャットとは違う調査補助価値があります。

ただし、GeminiにもCMP本文や依頼者情報は貼らない方針を明文化すべきです。
OSINTは公開情報を扱う一方、「何を調べているか」「誰のために調べているか」「どの案件のためか」という調査目的自体が機密になることがあります。

### Windsurf Max降格

Windsurf MaxからProへの降格は、Devin評価後に判断すべきです。
Windsurfではquotaや追加利用がモデルや使用量に応じて課金・消費されるため、Devinや高負荷タスクを実際に使わないとMaxの費用対効果は測れません。

私なら、今すぐPro降格はしません。
4週間だけ「Devin価値測定月」として使い切り、その後にMax継続かPro降格を決めます。

### Claude Max継続

Claude Max継続は妥当です。
先生の用途では、Claude Code、長文整理、Obsidian/Legal Pipeline、日中英の多言語文書処理、方針文書作成においてClaudeの比重が高いからです。

ただし、Claude Maxを「機密本文処理の安全装置」と見なしてはいけません。
Claude Maxは中核思考・設計・コード補助に使い、機密本文は可能な限りmetadata化・匿名化・人工承認済み抜粋化してから扱うべきです。

## 見落とし視点

### 1. オンプレLLM

文書の中で最も追加価値があるのは、Ollama等のローカルLLMの検討です。
精度はClaudeやGeminiに劣っても、ファイル名分類、PII候補検出、D-class候補スクリーニング、正規表現生成、簡易要約などには使える可能性があります。

ただし、オンプレLLMを導入しても「100%匿名化」は保証できません。
先生の既存方針どおり、AIによる脱敏は補助であり、最終判断は人間が行うべきです。

### 2. OSINTの機密性

OSINTは公開情報だから安全、という整理は不十分です。
公開情報そのものは非機密でも、調査対象、調査順序、検索語、依頼者の関心、リスク評価軸は機密になり得ます。

したがって、OSINT用にも分類が必要です。

| レベル | 例 | 使用可能AI |
|---|---|---|
| Public OSINT | 一般ニュース、政府発表、公開統計 | Gemini, Perplexity, Claude |
| Sensitive OSINT | 特定企業・特定案件に紐づく調査方針 | Claude/Windsurfでも要注意 |
| Privileged OSINT | 依頼者戦略・訴訟・交渉に直結 | 外部AI禁止、ローカル処理のみ |

### 3. GitHub Copilotの扱い

文書ではCopilotを主役にしていませんが、VS Code Audit profileにCopilotを入れない判断は正しいです。
GitHub Copilotは2026年6月1日からAI Creditsベース課金に移行し、Copilot Chat、CLI、cloud agent、Spaces、Spark、third-party coding agentsなどがクレジット消費対象になります。

先生の場合、Copilotを追加するとコストよりも「AI入口が増える」ことによる統制複雑化が問題になります。
したがって、現時点ではCopilotは導入不要です。

## 私の最終案

Claude案を少し修正し、以下を最終構成として提案します。

| 区分 | ツール | 判断 |
|---|---|---|
| 中核思考・文書設計 | Claude Max | 継続 |
| ローカル自動化 | Claude Code | 継続。ただし機密本文を読ませない設計 |
| 主AI IDE | Windsurf Max | 4週間評価後にMax/Pro判断 |
| クラウドPR型実行 | Devin.ai | 非機密allowlist repo限定 |
| 調査 | Gemini Advanced | Deep Research用途で継続 |
| 監査席 | VS Code Audit profile | AI拡張ゼロ、telemetry off |
| 補助 | Cursor | 課金停止、必要時再開 |
| 補助 | ChatGPT Plus | 1か月協議用に残すか、即解約 |

短く言えば、私の推奨はこうです。

> Claudeを頭脳、Windsurfを作業場、Devinを非機密PR作業員、Geminiを公開情報リサーチャー、VS Codeを監査席にする。CursorとChatGPTは原則整理対象。ただしChatGPTは多角協議が終わるまで1か月だけ残してもよい。

## 他AIに渡す前の修正文

この文書をChatGPTやGeminiに渡す前に、以下の5行を追加することを勧めます。

```markdown
補足：
本書でいう「ローカル処理」は、必ずしも「LLMへの外部送信ゼロ」を意味しない。
Claude Code等のローカルエージェントであっても、モデル推論のために情報が外部サービスへ送信され得る。
したがって、機密本文・依頼者名・案件戦略・D-class文書は、原則としてどの外部LLMにも入力しない。
許容されるのは、metadata、ファイル構造、人工承認済みの匿名化抜粋、または公開情報に限定する。
最終的な守秘判断は、AIルール文書ではなく、OS権限・workspace隔離・Git制御・人間承認で担保する。
```

この補足を入れるだけで、文書全体の安全性がかなり上がります。

本回答はAIによる一般的検討であり、個別案件の法的助言ではありません。実際の導入・運用・依頼者情報の取扱いについては、適格な専門家としてのご自身の判断および必要に応じた外部助言に基づいてください。連絡先: semin_tokyo@gopros.pro

---

## Redactions

- None. Operator confirmed `semin_tokyo@gopros.pro` is the operator's own
  law-office address inserted by Perplexity per a standing user instruction.

## Lead AI Annotation (Claude)

This response identified the in-group bias error in v1 (Claude Code as
"safe local processing") that the lead AI (Claude) had committed. The
4 modifications, the 5-line 補足 passage, and the OSINT 3-tier
classification were all accepted by the lead AI in full and are
incorporated into the v2 framework.
