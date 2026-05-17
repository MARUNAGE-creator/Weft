# Weft — Project Context for Claude Code

このファイルは Claude Code がセッション開始時に自動で読み込みます。Weft の開発を引き継ぐ際、ここに書かれた内容は **常にコンテキストに乗ります**。

詳しい背景は `docs/07-handover-for-new-thread.md` を必ず参照すること。

---

## プロジェクト概要

- **Weft** — クロスリサーチ型 note 記事生成 Web アプリ
- 単一 HTML ファイル(`app/weft-v0.1-step1.html`)で動作、バックエンド不要
- Gemini 2.5 Flash API のみ使用(Google Search Grounding 機能を活用)
- localStorage で API キーとプロジェクト履歴を保存

## 現在の状態

- **バージョン:** v0.11.1(ペルソナ独立セクション化 + 論文フィルタ maxTokens 再々増強、配布版・GitHub Pages 公開済み)
- **公開 URL:** https://marunage-creator.github.io/Weft/
- **リポジトリ:** https://github.com/MARUNAGE-creator/Weft(Public)
- **次のマイルストーン:** 実テスターからのフィードバック収集と対応 → モバイル最適化 or マネタイズ準備

## ファイル構成

```
weft/
├── CLAUDE.md                  ← このファイル(Claude Code 用)
├── README.md                  ← 人間用の入口
├── app/
│   └── weft-v0.1-step1.html  ← アプリ本体(編集対象は主にここ)
└── docs/
    ├── 01-blueprint.md
    ├── 02-changelog.md        ← バージョン履歴(変更時は必ず更新)
    ├── 03-setup-guide.md
    ├── 04-decisions.md        ← 重要判断の記録(決定時は必ず追記)
    ├── 05-roadmap.md
    ├── 06-security-notes.md
    ├── 07-handover-for-new-thread.md  ← 引き継ぎサマリー
    ├── 08-workflow-habits.md  ← 運用3原則
    ├── 09-market-research.md  ← 外部データの記録(note 相場など)
    └── 10-user-manual.md     ← ユーザー向け取扱説明書(v0.9.0 時点)
```

## ユーザー情報

- **名前:** SHIN
- **GitHub:** MARUNAGE-creator
- **環境:** Windows 11(ARM)
- **得意:** HTML/CSS/JS、Firebase、GitHub Pages
- **進め方の好み:** 反復対話型。決断前に複数案を提示してもらいたい。視覚・対話的なアウトプットを好む

## 既知の罠(必読)

### 1. API キーは絶対にチャットに貼らない / 貼らせない
過去に1回事故あり。SHIN さんが誤って貼った場合は即座に警告し、失効・再発行を促すこと。

### 2. `str_replace` ツールはテンプレートリテラル付近で頻繁に失敗する
特に `buildResearchPrompt()` `buildGenerationPrompt()` 周辺。失敗したら Python スクリプトを `/tmp/` などに作成して `content.replace()` で直接書き換える方が早い。

### 3. maxOutputTokens の罠を2回踏んでいる
プロンプトで出力量を増やす変更を入れる時は、必ず `maxOutputTokens` もセットで確認・調整すること。

現状の設定:
- `callGeminiGrounded`(リサーチ&分析):16384 トークン
- `callGeminiPlain`(本文生成):ユーザー選択(4096 / 8192 / 12288、`BODY_LENGTH_TOKENS` 参照)
- `callGeminiPlain`(タイトル再生成):2048

### 4. `finishReason: 'MAX_TOKENS'` の検知
両方の Gemini 呼び出しに実装済み。打ち切られた場合は UI に警告を出す設計。これを外さないこと。

## 重要な技術判断(変えないこと)

- **Gemini Search Grounding を採用**(Google Custom Search は 2025 年に新規受付停止のため使えない)
- **単一 HTML 構成を維持**(個人ツール段階。SaaS 化はまだ早い)
- **API キーは1個に保つ**(セットアップの簡便さ最優先)

詳細は `docs/04-decisions.md` を参照。

## 開発の3原則(SHIN さんと共有)

1. **重要な決定は明示する**(【DECISION】タグを会話で使う)
2. **バグ修正の理由を残す**(同じ罠を二度踏まない)
3. **諦めた案も記録する**(検討の重複を防ぐ)

これらは `docs/08-workflow-habits.md` で詳述。

## 作業時の運用ルール

### 重要な決定が出たら
→ `docs/04-decisions.md` に追記する形でユーザーに提示。決定タイトル / 状況 / 検討した選択肢 / 結論 / 副作用 の構造で。

### バージョンアップする時
→ HTML 内の `<span class="version-tag">` も更新する
→ `docs/02-changelog.md` の冒頭に新セクションを追加する
→ 重要な技術判断が絡んでいたら `04-decisions.md` にも追記

### Step 2 以降に進む時
→ `docs/05-roadmap.md` を先に参照すること(計画と方針が書いてある)
→ 既存の Step 1 機能を壊さないよう、新機能はトグル方式で実装することを検討

## デザイン規約

- **カラー:** `--indigo-deep #1a2238` / `--cream #f5f1ea` / `--madder #c9785b`
- **フォント:** Fraunces(見出し) / DM Sans(本文) / JetBrains Mono(メタ)
- **デザイン哲学:** artisan minimalist。装飾過多にしない、織物的な余白を活かす

## ブランド・トーン

- タグライン: 「横糸を、あなたの言葉に」
- ターゲット: note で知識・ノウハウを販売したい個人(D層)
- 差別化: AI 推測ではなく実データに基づく分析、待機時間の「視点取り込み」

## 直近の TODO(未確定)

- v0.1.6 の実運用テスト結果のフィードバック対応
- `parseTitles()` の正規表現が壊れた場合の調整
- Step 2(YouTube)の設計開始

---

**初回起動時のおすすめ:** SHIN さんに「v0.1.6 を試したか、何をやりたいか」を聞いてから動くこと。前のセッションの続きとは限らない。
