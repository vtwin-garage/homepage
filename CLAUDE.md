# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## これは何か

V TWINガレージマーケット(屋根付きブースの地域密着型フリマ・マルシェイベント)のための、ビルド不要・フレームワーク不要・パッケージマネージャ不要の1ページ静的サイトです。構成ファイルは3つのみです。

- `index.html` — ページの全コンテンツと構造
- `css/style.css` — 全スタイル
- `js/script.js` — フッターの年表示とモバイルナビの開閉のみ

## 実行・プレビュー方法

ビルド・lint・テストの仕組みはありません。`index.html` をブラウザで直接開く(または任意の静的ファイルサーバーでフォルダを配信する)だけで確認できます。

## アーキテクチャ

すべて `index.html` 内に、`<section id="...">` ブロックの一連の流れとして存在し、`css/style.css` の対応するクラスでスタイリングされています(例: `#vendor` セクションは `.vendor`、`.vendor-detail`、`.vendor-category` などと対応)。テンプレート機構は無く、繰り返し構造(feature card、schedule card、timeline item、FAQ item)は手作業でHTMLを複製しているため、共有コンテンツ(開催日、料金、ブース数など)を変更する際は複数箇所への反映が必要になることが多いです。セクションの並び順:

1. `header`(`.site-header`) — ロゴ+ナビ。モバイルナビは `script.js` 内の `#navToggle`/`#siteNav` で開閉
2. `#about` — 紹介文 + `.feature-grid` カード
3. `#schedule` — 開催日時(`.date-month`)と会場を示す `.schedule-card`。同じ日時の文字列が下部の `#access` にも重複しているので、両方を更新すること
4. `#vendor` — ブース数・料金(`.info-stats`、`.price-table`)、出店カテゴリ、LINE出店応募CTA
5. `#flow` — 当日のタイムライン(`.timeline`)
6. `#notice` — 注意事項を `.notice-group` カードでグループ化
7. `#faq` — `<details>`/`<summary>` によるアコーディオン
8. `#access` — 地図埋め込み + 重複した日時文字列
9. `footer`(`#contact`) — LINE問い合わせリンク。年表示は `script.js` が自動更新

### スタイリングの仕組み

`css/style.css` はサイト全体の配色とタイポグラフィのスケールを `:root` 内のCSSカスタムプロパティとして定義しています(`--color-primary`、`--color-primary-dark`、`--color-accent`、`--color-accent-soft`、`--color-text`、`--color-text-light`、`--color-bg`、`--color-card`、`--color-border`、`--shadow-soft`、および `--font-base`/`--font-heading`)。`--color-line`/`--color-line-dark` はLINEのブランドグリーンに固定されているため、配色を変更する際もこれらは変えないこと — サイト独自の配色を表すのはprimary/accent/text/background系の変数のみです。配色を変更する場合は、`:root` の外にあるハードコードされたhex/rgba値(グラデーション、バッジの文字色、テーブル/カテゴリの背景色、フッターの色など)も合わせて更新する必要があります。すべてが変数を参照しているわけではないため注意してください。

フォントはGoogle Fontsから読み込んでいます(見出しは `--font-heading` 経由で `Rounded Mplus 1c`、本文は `--font-base` 経由で `Noto Sans JP`)。

### コンテンツに関する注意点

- LINE公式アカウントのリンク(`https://line.me/R/ti/p/%40019tayoi`)は複数のCTA(ヒーロー、出店案内セクション、フッター)に登場するので、変更時は同期させること。
- 開催日時、ブースサイズ、料金は `#schedule`、`#vendor`、`#access` にまたがって重複しているため、更新時は1箇所だけでなくファイル全体を旧の値でgrepして反映すること。
