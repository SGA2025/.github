# 多角AI協議用文書：法律実務家のためのAIスタック設計

**起案日**: 2026年5月10日
**起案者**: Claude（Anthropic / Opus 4.7）
**目的**: 本文書を ChatGPT・Gemini など他AIに提示し、それぞれの見解・反論・補完案を得ること。三方間以上の対話を通じて、依頼者にとって最善のAIスタック構成を確定させる。
**読者想定**: 本会話の文脈を持たない他AIモデル。文脈なしで読んで判断できるよう自己完結型に記述する。

---

## 第1部 — 背景と前提

### 1.1 依頼者プロファイル

- **職業**: 日本の法律実務家（弁護士業務に類する守秘義務を負う立場）
- **業務領域**:
  - CMP（具体名は秘匿）と呼ばれる法律案件群。守秘性が極めて高く、依頼者氏名・案件番号・本文・戦略情報を含む。
  - Obsidian Vault による知識管理。Vault A は機密寄り、Vault B は非機密。
  - OSINT（公開情報からの調査・分析）の自動化。
  - 自身のウェブサイト・GitHub Pages の軽微な保守。
- **作業環境**: macOS（MacBook）。GitHub・Obsidian・複数のAI IDE を併用。
- **法的制約**:
  - 弁護士法23条（守秘義務）
  - 個人情報保護法
  - 依頼者との委任契約上の守秘義務
  - これらにより、**依頼者情報を外部AIサービスのクラウドへ送信することは原則として違反リスクを伴う**。

### 1.2 現在契約中のAIサービスと月額

| サービス | プラン | 月額目安(USD) | 備考 |
|---|---|---:|---|
| Claude（Anthropic） | Max | $100〜200 | Claude Code 含む |
| Windsurf | Max | $200 | Devin Cloud 統合済 |
| Cursor | Pro | $20 | 直近1か月ほぼ未使用と本人確認 |
| ChatGPT | Plus | $20 | 用途は「一般会話・質問」のみと本人確認 |
| Gemini | Advanced | $20 | Deep Research を月3〜5回利用 |
| Google Workspace | Business（AIなし） | 別途 | Gemini Advanced は独立契約 |
| **合計（最大）** | | **約$360〜480** | 年$4,300〜5,800 |

### 1.3 すでに合意されている運用原則

依頼者と起案AI（Claude）の間で議論を経て、以下は前提として確定済み:

1. **機密データは原則としてMacBookから外部へ送信しない**。
2. **AIエージェント用のルール文書（AGENTS.md / CLAUDE.md / `.windsurf/rules` / `.cursor/rules`）は、それ自体が"境界"ではなく、参考資料に過ぎない**。法的境界は OS・Git・workspace隔離など技術的手段で物理的に強制する。
3. **「監査席」の概念**: AI を完全に無効化したエディタを別個に持ち、AIが書いた差分・機密文書をそこで確認する。AIが書いたものをAIだけで承認しない。
4. **共通ルールの一元管理**: `AGENTS.md`（または `ai_policy/MASTER_BOUNDARY_RULES.md`）を正本とし、各IDEには薄いアダプターのみを置く。Claude Code は AGENTS.md をネイティブに読まないため `ln -s AGENTS.md CLAUDE.md` で対応する。

---

## 第2部 — 検討経緯（要約）

### 2.1 第1議題：AGENTS.md による IDE ルールの統一管理

依頼者は当初「Windsurf と Cursor で別々のルール本文を管理するのは非効率」という認識を持ち、`AGENTS.md` を共通正本とする三層構成（`AGENTS.md` / IDE別アダプター / `ai_policy/` 機械可読制御ファイル）を提案。

**起案AIの見解**:
- 設計の方向性は正しく、業界ベストプラクティスに沿っている。
- ただし**Claude Code は2026年4月時点で AGENTS.md をネイティブに読まない**（Anthropic GitHub Issue #6235 が未解決）。CLAUDE.md → AGENTS.md のシンボリックリンクが標準的回避策。
- より根本的に、**ルール文書は LLM への単なるプロンプト追加であり、法的境界には不十分**。CMP案件のような特権情報には:
  1. ファイルシステム権限による隔離
  2. IDE workspace に D-class ディレクトリを含めない
  3. `.gitignore` と pre-commit フックによる機械的ブロック
  4. AIルール文書（第4層・最弱）
  という多層防御が必要。

### 2.2 第2議題：VS Code の追加検討

依頼者は VS Code を「AI と競争する第3のエディタではなく、監査・標準確認用の中立エディタ」として追加することを提案。

**起案AIの見解**:
- 「監査席」の概念は鋭い洞察。多層防御の第2層と完全に整合。
- ただし事実誤認: **VS Code はテレメトリがデフォルトで有効**。Windsurf の方が paid plan で zero data retention が標準。CMP用途では `telemetry.telemetryLevel = "off"` を明示設定する必要あり。
- **Claude Code は2026年初に Anthropic 公式 VS Code 拡張機能 が GA**。Windsurf にも拡張がインストールできるため、「Claude Code のために別途VS Codeを立てる」必要はない。
- VS Code は **Audit profile 1つだけ**作る（AI拡張ゼロ・テレメトリOFF）のが正解。AI-Dev profile は Windsurf と重複するため不要。

### 2.3 第3議題：Devin Cloud の評価

依頼者は Windsurf Max に統合された Devin Cloud を構成に組み込み、「PRレビュー可能な非機密タスク限定」で運用する方針を提案。

**起案AIの見解**:
- 「機密タスクは Devin に投げない」という原則設定は正しい。
- ただし日本の弁護士法23条・個人情報保護法を踏まえると、より厳格な**ホワイトリスト方式**にすべき。`ai_policy/devin_allowlist.md` で「Devin に渡してよいリポジトリ」を明示し、それ以外は禁止。
- Devin の品質: 2026年の複数レビューが "quality inconsistent" と指摘。Cognition自身が「コードレビューがボトルネック」と認め Devin Review を後付けで開発。**Devin の PR は branch protection で必須レビュー**にすべき。
- コスト: Windsurf Max($200) は Devin 使用が前提。月$50相当の overage が初回付与されるが、長時間タスクで容易に枯渇。

### 2.4 第4議題：重複ツールの整理

依頼者からの問題提起：「5つのAI（Windsurf / Devin / Claude Code / VS Code / Cursor）すべてを使う必要はない」。

**起案AIの見解と依頼者の確認結果**:
- **Cursor**: 直近1か月「ほぼ使っていない」と確認 → **解約推奨**。Windsurf と機能重複。
- **VS Code AI-Dev profile**: 不要（Windsurf と重複）。
- **VS Code Audit profile**: 必須（他に代替なし）。
- **Devin**: 月3〜5件の非機密PR型タスクがあると確認。**1か月間 Devin 不使用のAテストで要否を実証**することを推奨。
- **Windsurf Max**: Aテストの結果次第で **Pro($20) に降格**することで月$180節約可能。
- **Claude Code**: 機密ローカル処理の中核として継続。

### 2.5 第5議題：ChatGPT・Gemini の評価

依頼者から「GPT と Gemini も契約している」と追加情報。

**確認結果**:
- ChatGPT Plus の用途は「一般会話・質問のみ」 → Claude.ai で完全代替可能。
- Gemini Advanced で **Deep Research を月3〜5回**利用 → Claude にない差別化価値が出ている。
- Google Workspace は契約中だが AI add-on なし → Gemini Advanced は独立契約として評価。

**起案AIの見解**:
- ChatGPT Plus: **解約推奨**。
- Gemini Advanced: **継続推奨**。Deep Research は OSINT・法律調査で genuine に強力。
- ただしどちらも、Gemini にも CMP 案件本文を貼らないという機密境界を `ai_policy/MASTER_BOUNDARY_RULES.md` に明記する必要あり。

---

## 第3部 — Claude（起案AI）の最終推奨構成

### 3.1 残すべき契約

| サービス | 月額 | 役割 |
|---|---:|---|
| **Claude Max（Claude Code含む）** | $100〜200 | 機密ローカル処理・一般会話・要約・コード作業 |
| **Windsurf**（Pro $20 or Max $200） | 変動 | 非機密 IDE 作業。Aテスト後に Pro 降格判定 |
| **Gemini Advanced** | $20 | Deep Research（OSINT・法律調査・公開資料分析） |
| **Google Workspace** | 既存 | メール・ドライブ・カレンダー |

### 3.2 解約推奨

| サービス | 月額削減 | 根拠 |
|---|---:|---|
| **Cursor Pro** | -$20 | 実用頻度ゼロ |
| **ChatGPT Plus** | -$20 | 用途が Claude と完全重複 |

### 3.3 条件付き判断

| 項目 | 判定方法 |
|---|---|
| **Devin Cloud（Windsurf Max）** | 1か月不使用テスト → 困った回数で要否判定 |
| **Windsurf Max → Pro 降格** | 上記Aテストの結果に連動 |

### 3.4 設計原則（4箇条）

1. **データがMacBookから外部に出るか否かを、すべての判断の最初の軸とする。**
2. **機密案件は Claude Code（ローカル処理）のみ。** Devin・Cursor・GPT・Gemini に CMP 関連データを送信してはならない。
3. **AIが生成した出力は、別の AI ではなく VS Code Audit profile（AI完全無効）で目視確認する。**
4. **ルール文書（AGENTS.md等）は第4層防御に過ぎない。**OS権限・Git・workspace隔離・pre-commit フックを優先する。

### 3.5 月額コスト効果

| シナリオ | 月額 | 年額 | 削減幅 |
|---|---:|---:|---:|
| 現状（Cursor + ChatGPT 契約中） | $360〜480 | $4,320〜5,760 | — |
| Cursor + ChatGPT 解約 | $320〜440 | $3,840〜5,280 | -$480/年 |
| 上記 + Windsurf Max → Pro | $140〜260 | $1,680〜3,120 | -$2,640/年 |

---

## 第4部 — 起案AIが下した判断の根拠

### 4.1 弁護士法23条と LLM API 送信の整合性

弁護士法23条は依頼者の秘密を保持する義務を弁護士に課す。Anthropic・OpenAI・Google・Cognition などのLLMサービスはいずれも、API リクエスト時にデータが各社のサーバーを通過する。Pro/Enterprise 契約で zero data retention が宣言されていても、**通過すること自体**が「第三者への開示」と解釈されうるリスクがある。

**起案AIの結論**: 仮に zero retention が技術的に履行されていても、依頼者本人の事前同意なく依頼者情報を外部 LLM に送信するのは保守的回避が望ましい。Claude Code であっても、**指示と差分のみを送り、機密文書本体を送らない運用**にできる。

### 4.2 Devin が他のAIと質的に異なる理由

Claude API・Gemini API は「単発の推論」を行う。Devin Cloud は「**自律的に複数ステップの実装・テスト・PR作成を行う非同期エージェント**」であり、サンドボックスとはいえ Cognition 管理下の VM 内でファイル操作・コマンド実行・ネットワーク通信が走る。これは API 呼び出しとは別次元の信頼を要求する。

依頼者のような立場では、Devin に与えるリポジトリは**ホワイトリスト方式で物理的に限定**するのが合理的。

### 4.3 監査席を独立させる必要性

Windsurf 内で生成された差分を Windsurf 内のレビュー UI で確認すると、同じ AI が書いたコメントや要約が判断を bias する。**生成系統と検証系統を物理的に分離**することで、認知的独立性が保たれる。VS Code Audit profile はこの「検証専用環境」として機能する。

### 4.4 重複の整理が機密保持を強化する

AI ツールが多いほど:
- 設定ミスの確率が増える
- どのツールがどのデータを読んだか追跡できなくなる
- ルール文書のドリフトが起こる
- テレメトリの抜け漏れが発生しやすい

**ツール削減はコスト削減ではなく、機密保持の強化**として捉える。

---

## 第5部 — 他AIへの諮問事項

本文書を読んだ ChatGPT・Gemini・その他のAIに対し、起案AI（Claude）から以下を求める。

### 5.1 起案AIの推奨に対する反論・補強

以下のいずれかについて、明確な反対意見・修正提案・別案があれば述べてほしい。

1. **Cursor 解約の妥当性**: 起案AIは「実用頻度ゼロ」を理由に解約を推奨したが、Cursor 固有の優位性（Composer の多ファイル編集、特定モデルへのアクセス等）で再評価すべき点はないか。
2. **ChatGPT Plus 解約の妥当性**: 起案AIは「Claude.ai で完全代替可能」と判断したが、ChatGPT Plus（または Pro）に固有で、依頼者プロファイルに価値を提供しうる機能（カスタム GPTs、Operator、Sora、Advanced Voice等）はないか。
3. **Gemini Advanced 継続の妥当性**: Deep Research 以外に依頼者プロファイルで活きる機能（NotebookLM 統合、2M context、画像生成等）はあるか。逆に Deep Research は Claude や ChatGPT で代替可能ではないか。
4. **Devin Cloud の Aテスト方針**: 1か月の不使用テストは合理的か。それとも別の評価方法（特定タスクで A/B テスト等）が適切か。
5. **Windsurf Max → Pro 降格**: Devin 以外で Max に固有の価値（より大きい context、優先サポート等）はあるか。
6. **Claude Max の選定**: 競合サービス（GitHub Copilot、Cody、Continue 等）に切り替えた方が依頼者のニーズに合う可能性はあるか。

### 5.2 各AIの自己評価

ChatGPT および Gemini に対しては、**自社サービスの強みと弱みを率直に**述べてほしい:

- 自社サービスが、依頼者プロファイルにおいて **Claude より優れている領域**は何か。
- 自社サービスが、依頼者プロファイルにおいて **Claude に劣る領域**は何か。
- 機密データを送信しないという制約下で、自社サービスはどこまで価値を出せるか。

この質問は自己宣伝を求めているのではなく、**自己批判を含む正直な評価**を求める。

### 5.3 起案AIが見落とした視点

以下のような観点で、起案AIの分析が不足している可能性がある:

- **macOS ネイティブの統合**（Apple Intelligence、Mail、Notes、Spotlight等）の活用
- **オンプレミス LLM**（Ollama 等）でローカル機密処理を完全に外部送信ゼロにする選択肢
- **エンタープライズ契約**（Anthropic Enterprise、Google Workspace AI、Cognition Enterprise）への切り替え可能性
- **Windsurf 以外のIDE**（Zed、Cody、JetBrains AI Assistant 等）の評価

これらについて、依頼者プロファイルとの適合性を評価してほしい。

### 5.4 三方間で議論したい論点

最も生産的に議論できそうな論点を起案AIから提示する:

1. **「ルール文書 vs 技術的境界」の重み付け**: 起案AI は技術的境界を強く推奨したが、ルール文書も実務上は十分機能する場面がある。両者のバランスについて。
2. **Devin のような自律エージェントを法律実務家が使う倫理的枠組み**: 業務上の責任所在、PR レビュー基準、監査ログ要件。
3. **AGENTS.md vs CLAUDE.md vs `.github/copilot-instructions.md` の長期的な収束予測**: どの規格が標準化するか、そしてそれは依頼者の運用にどう影響するか。
4. **OSINT 自動化と機密境界の交差点**: OSINT は公開情報を扱うが、ターゲットや調査目的が機密になる。この境界をどう設計するか。

---

## 第6部 — 出典と検証された事実

起案AI が判断の根拠とした主要な事実と出典:

### 6.1 ツール仕様の検証済み事実

- **Claude Code は AGENTS.md を ネイティブに読まない（2026年4月時点）**: [GitHub Issue #6235](https://github.com/anthropics/claude-code/issues/6235)。シンボリックリンク `ln -s AGENTS.md CLAUDE.md` が公式コミュニティで推奨されている回避策。
- **Claude Code 公式 VS Code 拡張は2026年初に GA**: [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)。Windsurf 等のVS Code フォークにもインストール可能。
- **Windsurf AGENTS.md サポート**: [Windsurf Docs](https://docs.windsurf.com/windsurf/cascade/agents-md)。ルートに置けば always_on ルール扱い。
- **Windsurf ルールの文字数制限**: 個別ファイル12,000字、Global 6,000字、超過時はサイレントドロップ。
- **Cursor の `AGENTS.md` と `.cursor/rules` の優先順位**: [Cursor Docs](https://cursor.com/docs/rules)。競合時は `.cursor/rules` が優先。
- **VS Code はテレメトリがデフォルト ON**: 公式設定で `telemetry.telemetryLevel = "off"` 必須。
- **GitHub Copilot 2026年6月1日から AI Credits ベース課金に移行**: [GitHub Blog](https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/)。
- **Devin Cloud は Cognition のクラウド sandbox で動作**: [Devin Security](https://devin.ai/security)。Pro/Enterprise で zero retention。
- **Windsurf March 2026 価格改定**: Pro $20、Max $200。Devin Cloud は self-serve plan に統合済。

### 6.2 法的・倫理的根拠

- 弁護士法23条（守秘義務）
- 個人情報保護法
- 各 LLM サービスの利用規約・データ取扱方針

### 6.3 起案AIの認識限界

起案AI（Claude Opus 4.7）は2026年1月までの学習データに基づき、それ以降の情報は本文書作成時点での Web 検索により補完した。以下については認識が不確実な可能性がある:

- 各サービスの2026年5月時点の最新仕様
- 日本における弁護士業務での AI 利用に関する最新の判例・ガイドライン
- 依頼者の業務実態の詳細（CMP の具体的内容は秘匿、起案AI も尋ねていない）

---

## 第7部 — 他AIへの依頼事項（要約）

本文書を読む他AIに対する具体的依頼:

1. 第3部「最終推奨構成」のうち、**異論がある項目を具体的に挙げ**、その根拠とともに反対意見を述べてほしい。
2. 第5部「諮問事項」の各項目に対し、**自社サービスを含めて率直に評価**してほしい。
3. 起案AI が**見落としている観点**があれば、優先度の高い順に3つ挙げてほしい。
4. 依頼者の業務継続性とコスト最適化のバランスについて、**具体的な代替案**を1〜2つ提案してほしい。

---

## 第8部 — 議論の継続方法

依頼者は本文書を:
1. ChatGPT に提示し、ChatGPT の見解を取得する。
2. Gemini に提示し、Gemini の見解を取得する。
3. それらの見解を Claude（起案AI）に再度持ち込み、**起案AIが反論または受容**する。
4. 必要に応じてこのサイクルを複数回実施する。

**期待される成果**:
- 単一AIの偏向（自社びいき・楽観バイアス・アーキテクチャの好み）を相互に補正する。
- 各AIが独立に検証することで、依頼者の意思決定の質を高める。
- 依頼者の業務に最も適合した、AI スタックの最終構成を確定させる。

起案AI は、他AIの見解を**敵対的・防御的にではなく、協力的に**受け入れる準備がある。明確な反論には立場を変える可能性があり、その場合は変更点を明示する。

---

**起案AI（Claude / Anthropic / Opus 4.7）から他AIへ**:

本文書はあなたへの挑戦ではなく、依頼者という共通の利害関係者を支援するための共同作業の招待状である。私の推奨に賛成する必要はない。あなたの正直な評価が、依頼者にとって最も価値ある贈り物となる。

以上。
