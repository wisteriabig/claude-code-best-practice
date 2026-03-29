# claude-code-best-practice
practice makes claude perfect

[English](README.md) | 日本語

## このリポジトリについて

**claude-code-best-practice** は、Anthropic が開発した AI コーディングアシスタント [Claude Code](https://code.claude.com/) を最大限に活用するためのベストプラクティスをまとめたリファレンスリポジトリです。

サブエージェント・コマンド・スキル・フック・ワークフローなどの機能について、実際に動作するサンプル実装と共に、実践的な知識を体系的に整理しています。Boris Cherny（Claude Code 創設者）や Anthropic チームのヒント・トリック 86 件を集約しており、日々の開発で Claude Code を使いこなすための出発点となります。

---

## 🧠 主要コンセプト

| 機能 | 場所 | 説明 |
|------|------|------|
| **サブエージェント** | `.claude/agents/<name>.md` | 独立したコンテキストで動作する自律エージェント。カスタムツール・権限・モデル・メモリを個別に設定可能 |
| **コマンド** | `.claude/commands/<name>.md` | スラッシュコマンドとして呼び出せるプロンプトテンプレート。既存コンテキストに知識を注入するワークフロー用 |
| **スキル** | `.claude/skills/<name>/SKILL.md` | 設定可能・プリロード可能・自動検出可能な知識ユニット。コンテキストフォークやプログレッシブディスクロージャーに対応 |
| **ワークフロー** | `.claude/commands/weather-orchestrator.md` | **コマンド → エージェント → スキル** のオーケストレーションパターンの実装例 |
| **フック** | `.claude/hooks/` | 特定のイベントに対してスクリプト・HTTP・プロンプト・エージェントを実行するユーザー定義ハンドラ |
| **MCP サーバー** | `.claude/settings.json`, `.mcp.json` | 外部ツール・データベース・API への Model Context Protocol 接続 |
| **設定** | `.claude/settings.json` | 階層型設定システム（管理設定 > プロジェクト設定 > 個人設定） |
| **メモリ** | `CLAUDE.md`, `.claude/rules/` | `CLAUDE.md` ファイルと `@path` インポートによる永続コンテキスト |
| **チェックポインティング** | 自動（git ベース） | ファイル編集の自動追跡。`Esc Esc` または `/rewind` で元に戻せる |

---

## 🔀 オーケストレーションワークフロー

**コマンド → エージェント → スキル** の連携パターンをウェザーアプリで実演しています。

```
/weather-orchestrator コマンド
  └─ weather-agent（気温取得）
       └─ weather-fetcher スキル（Open-Meteo API）
  └─ weather-svg-creator スキル（SVG カード生成）
```

**試し方:**

```bash
claude
/weather-orchestrator
```

詳細は [orchestration-workflow/orchestration-workflow.md](orchestration-workflow/orchestration-workflow.md) を参照してください。

---

## ⚙️ 開発ワークフロー

主要ワークフローはすべて **調査 → 計画 → 実行 → レビュー → リリース** の共通パターンに従います。

| ワークフロー | 特徴 |
|-------------|------|
| [Superpowers](https://github.com/obra/superpowers) | TDD ファースト・Iron Laws・全体計画レビュー |
| [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) | 本能スコアリング・AgentShield・多言語ルール |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | フル SDLC・エージェントペルソナ・22 以上のプラットフォーム対応 |
| [Get Shit Done](https://github.com/gsd-build/get-shit-done) | フレッシュ 200K コンテキスト・ウェーブ実行・XML プラン |

その他: [クロスモデルワークフロー](development-workflows/cross-model-workflow/cross-model-workflow.md) · [RPI](development-workflows/rpi/rpi-workflow.md) · Ralph Wiggum Loop

---

## 💡 ヒントとコツ（抜粋）

### プロンプト
- 「これを証明して」「エレガントな解決策を最初から実装して」など、Claude に挑戦させる
- バグを貼り付けて「fix」とだけ言う。修正方法を細かく指示しない 🚫👶

### 計画・スペック
- 常に[プランモード](https://code.claude.com/docs/en/common-workflows)から始める
- 仕様を詳細に書き、曖昧さを排除してから作業を渡す
- 第 2 の Claude にスタッフエンジニアとしてプランをレビューさせる

### CLAUDE.md
- ファイルあたり [200 行以下](https://code.claude.com/docs/en/memory#write-effective-instructions)を目標にする
- 「`npm run test` を実行する」などの基本コマンドを必ず記載する
- `settings.json` で決定論的に制御できる設定は CLAUDE.md に書かない

### エージェント
- 「サブエージェントを使って」と言うだけで、より多くの計算リソースを問題に投入できる 🚫👶
- 独立したコンテキストウィンドウがより良い結果を生む（テスト時間コンピュート）

### Git / PR
- PR は小さく・集中した内容に。1 PR = 1 機能
- 常に[スカッシュマージ](tips/claude-boris-2-tips-25-mar-26.md)でクリーンなリニア履歴を維持
- 少なくとも 1 時間に 1 回はコミットする

### デバッグ
- 詰まったときはスクリーンショットを撮って Claude に共有する
- バックグラウンドタスクとしてターミナルを実行し、ログを見やすくする

---

## 📊 レポート

| レポート | 内容 |
|---------|------|
| [Agent SDK vs CLI](reports/claude-agent-sdk-vs-cli-system-prompts.md) | SDK とCLI のシステムプロンプトの違い |
| [Browser Automation MCP](reports/claude-in-chrome-v-chrome-devtools-mcp.md) | Claude in Chrome vs Chrome DevTools MCP |
| [Global vs Project Settings](reports/claude-global-vs-project-settings.md) | グローバル設定とプロジェクト設定の使い分け |
| [Agent Memory](reports/claude-agent-memory.md) | エージェントメモリのスコープと設定 |
| [Agents vs Commands vs Skills](reports/claude-agent-command-skill.md) | 3 つの機能の使い分けガイド |
| [Usage & Rate Limits](reports/claude-usage-and-rate-limits.md) | 使用量と料金制限 |

---

## 🚀 使い方

```
1. このリポジトリをコースとして読む。
   コマンド・エージェント・スキル・フックが何かを理解してから使い始める。

2. リポジトリをクローンして例を試してみる。
   /weather-orchestrator を実行し、フックの音を聴き、エージェントチームを動かしてみる。

3. 自分のプロジェクトで Claude に「このリポジトリを参考に、適用できるベストプラクティスを提案して」と頼む。
```

---

## 📁 ディレクトリ構成

```
claude-code-best-practice/
├── .claude/
│   ├── agents/          # サブエージェント定義
│   ├── commands/        # スラッシュコマンド
│   ├── skills/          # スキル定義
│   ├── hooks/           # フックスクリプト
│   ├── rules/           # ルールファイル
│   └── settings.json    # プロジェクト設定
├── best-practice/       # 各機能のベストプラクティス文書
├── implementation/      # 実装例
├── orchestration-workflow/  # ウェザーアプリのオーケストレーション例
├── reports/             # 調査レポート
├── tips/                # Boris Cherny 等のヒント集
├── development-workflows/   # 開発ワークフロー比較
├── tutorial/            # チュートリアル
└── CLAUDE.md            # メモリ・ルール設定
```

---

## 関連リポジトリ

- [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) — フックの実装例
- [codex-cli-best-practice](https://github.com/shanraisshan/codex-cli-best-practice) — Codex CLI のベストプラクティス
- [codex-cli-hooks](https://github.com/shanraisshan/codex-cli-hooks) — Codex CLI フック

---

*このリポジトリは Claude Code 自身によって継続的に更新されています。*
