# Weft — クロスリサーチ note 記事生成エンジン

> 横糸を、あなたの言葉に

## このフォルダの中身

```
weft/
├── CLAUDE.md                         Claude Code 用の自動読み込みコンテキスト
├── README.md                         このファイル
├── app/
│   └── weft-v0.1-step1.html         実際のアプリ(ブラウザで開く)
└── docs/
    ├── 01-blueprint.md               プロダクトの全体設計・ビジョン
    ├── 02-changelog.md               バージョン履歴(v0.1.0 → v0.1.6)
    ├── 03-setup-guide.md             API キー取得、起動、デプロイ手順
    ├── 04-decisions.md               重要な技術判断の記録
    ├── 05-roadmap.md                 Step 2 以降のロードマップ
    ├── 06-security-notes.md          API キー取り扱いなど
    ├── 07-handover-for-new-thread.md  新しい Claude スレッドへの引き継ぎ用
    ├── 08-workflow-habits.md         Weft 開発の3原則(運用ルール)
    ├── 09-market-research.md         外部データ調査の記録(note 相場 等)
    └── 10-user-manual.md             ★ ユーザー向け取扱説明書(テスター配布用)
```

## Weft 開発の3原則

**オーナー自身が長期的にプロジェクトを育てるための習慣。**

1. **重要な決定は明示する** — 「【DECISION】これは引き継ぎ対象」と Claude に伝える
2. **バグ修正の理由を残す** — 同じ罠を踏まない(maxOutputTokens の罠を2回踏んだ反省)
3. **諦めた案も記録する** — 「なんで採用しなかったんだっけ」を防ぐ

詳細・実践方法は `docs/08-workflow-habits.md` 参照。

## 3行で言うと

- **Weft = note.com を実データで検索しながら、記事の戦略分析と執筆を AI に任せるツール**
- **競合分析(パターン抽出 / 空きスポット発見)→ タイトル選択 → 本文生成** の3段階
- **「待機中に視点を仕込む」設計**で、生成物が AI 風の凡庸記事にならない

## クイックスタート(初めて使うとき)

1. **API キーを取得する**
   - https://aistudio.google.com/app/apikey にアクセス
   - Google アカウントでログイン → "Create API key"
   - `AIza...` で始まる文字列をコピー

2. **アプリを起動する**
   - `app/weft-v0.1-step1.html` をブラウザにドラッグ&ドロップ
   - または GitHub Pages にアップロード(`docs/03-setup-guide.md` 参照)

3. **設定する**
   - 「① API 設定」を開く
   - Gemini API キーを貼り付け → 保存

4. **使う**
   - テーマ入力 → リサーチ&分析 → タイトル選択 → 本文生成
   - 詳細は `docs/01-blueprint.md` 参照

## 重要な注意

**Gemini API キーは絶対にチャットに貼らない**(Claude との会話含む)。詳細は `docs/06-security-notes.md`。

## 次のセッションで Claude と会話する場合

`docs/07-handover-for-new-thread.md` の内容を最初のメッセージにコピペしてください。Claude が文脈を即座に取り戻せます。

## バージョン

現在:**v0.9.1**(論文・知恵袋を本文プロンプトに統合 / 重大バグ修正)

## ライセンス / 利用について

個人プロジェクト用に作成。
