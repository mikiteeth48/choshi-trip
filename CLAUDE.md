# CLAUDE.md — 銚子旅行プロジェクト

このファイルは Claude Code がセッション開始時に自動で読み込む「プロジェクト記憶」です。
作業前にここを確認してから実装・更新を行ってください。

---

## プロジェクト概要

**名称**: 銚子旅行プラン 2026 — 柏発 日帰りガイド  
**公開URL**: https://mikiteeth48.github.io/choshi-trip/  
**リポジトリ**: https://github.com/mikiteeth48/choshi-trip  
**形式**: 単一HTMLファイル（CSS・JS込み。外部ライブラリ不使用・ビルド不要）

---

## ファイル構成

```
choshi-trip/
├── index.html        ← サイト本体（唯一の編集対象）
├── CLAUDE.md         ← このファイル（Claude の記憶）
├── .env              ← GitHubトークン（Git管理外・公開禁止）
├── .gitignore        ← .env / .DS_Store を除外
└── docs/
    └── plan.md       ← 実装記録・フェーズ詳細・変更履歴
```

**注意**: `.env` は絶対に GitHub にプッシュしない（.gitignore で除外済み）。

---

## デプロイ手順（ファイル変更後は毎回これを実行）

```bash
cd /Users/akihiromiki/Downloads/choshi-trip

# 1. ステージング＆コミット（メッセージは内容に合わせて変える）
git add index.html
git commit -m "変更内容を一言で"

# 2. プッシュ（tokenはremote URLに埋め込み済みなので認証不要）
git push origin main
```

GitHub Pages が自動で再デプロイ（約1〜2分後に反映）。

### トークン期限切れ・認証エラーが出た場合

1. https://github.com/settings/tokens/new でトークンを新規発行（scope: `repo`）
2. `.env` の `GITHUB_TOKEN=` を新しいトークンに書き換え
3. `source .env && git remote set-url origin "https://${GITHUB_TOKEN}@github.com/${GITHUB_USER}/${GITHUB_REPO}.git"`

---

## デザインシステム（CSS変数）

```css
:root {
  /* ページ背景 */
  --bg:          #ffffff;    /* 基本白 */
  --bg-warm:     #faf8f5;    /* プランカード本文 */
  --bg-section:  #f2efe9;    /* トレインバー・ハイライト・フッター */

  /* テキスト */
  --ink:         #1d2d3e;    /* メイン */
  --ink-sub:     rgba(29,45,62,.60);  /* サブ */
  --ink-light:   rgba(29,45,62,.40);  /* 注釈 */

  /* アクセント */
  --teal:        #2a8b8c;    /* リンク・ボタン・アイコン */
  --foam:        #3aa0a1;    /* タイムライン時刻・イタリック体 */
  --sand:        #9e6c2a;    /* 価格・費用 */

  /* ボーダー */
  --bdr:         rgba(29,45,62,.11);
  --bdr-soft:    rgba(29,45,62,.07);
}
```

**注意**: ヒーローセクション（`.hero`）だけは暗い写真背景で固定。
ヒーロー内のテキストは `--hero-text: #f4f0e8` / `--hero-muted` を使用。

---

## HTML セクション構成

| セクション | ID / クラス | 背景色 |
|-----------|-----------|--------|
| スティッキーナビ | `#sticky-nav` | 白（半透明blur） |
| ヒーロー | `.hero` | 写真＋暗いオーバーレイ（変更禁止） |
| トレインバー | `.train-bar` | `--bg-section` |
| プラン3枚 | `#plan-1` `#plan-2` `#plan-3` | 白 / `--bg-warm` |
| 食事ガイド | `#food` | `--bg-warm` |
| 見どころ | `#highlights` | `--bg-section` |
| フッター | `footer` | `--bg-section` |

---

## 実装済み機能

| 機能 | 概要 |
|------|------|
| レスポンシブ | 768px・480px ブレークポイント |
| スティッキーナビ | HEROを越えたら表示（IntersectionObserver） |
| Google Maps リンク | タイムライン各スポットに 📍 |
| 時刻表リンク | トレインバー・フッターに pill ボタン |
| ライトボックス | 写真クリックで拡大 / サムネイルはグループ◀▶ |
| 詳細モーダル | 各プランの費用・タイムライン・アドバイス |
| 食事カード写真 | 各カード上部に銚子の景観写真＋店舗リンク |

### z-index 階層

| レイヤー | z-index |
|---------|---------|
| スティッキーナビ | 7000 |
| 詳細モーダル | 8000 |
| ライトボックス | 9000 |

---

## 食事セクション — 実店舗リンク

| カード | 店舗 | 公式URL |
|-------|------|---------|
| 地魚の握り寿司 | 銚子丸 | https://www.choushimaru.co.jp/ |
| ぬれ煎餅 | 銚子電鉄 公式 | https://www.choshi-dentetsu.jp/ |
| 銚子の醤油 | ヤマサ醤油 | https://www.yamasa.com/ |
| 銚子の醤油 | ヒゲタ醤油 | https://www.higeta.co.jp/ |
| 干物・いわし・ビール | 銚子市観光協会 | https://www.choshi-kankou.jp/ |

---

## 写真クレジット（Wikimedia Commons）

| ファイル名 | 被写体 | ライセンス |
|-----------|--------|-----------|
| `Sunrise_in_Inubosaki_(9027196940).jpg` | 犬吠埼の日の出 | CC BY 2.0 / Guilhem Vellut |
| `Choshi_Inubosaki_Lighthouse_2013-09C.JPG` | 犬吠埼灯台 | CC BY-SA 3.0 / 小石川人晃 |
| `Rising_sun_and_a_lighthouse_-_panoramio.jpg` | 灯台パノラマ | Wikimedia Commons |
| `Choshi_Electric_Railway_Line.JPG` | 銚子電鉄 | Wikimedia Commons |
| `Choshi_Fishing_Port_01.jpg` | 銚子漁港① | CC BY-SA 4.0 / 掬茶 |
| `Choshi_Fishing_Port_02.jpg` | 銚子漁港② | CC BY-SA 4.0 / 掬茶 |
| `InubosakiLighthouse.JPG` | 犬吠埼灯台（別角度） | Wikimedia Commons |

---

## よくある変更パターン

### テキストを変える
→ `index.html` 内の該当テキストを Edit ツールで変更 → コミット＆プッシュ

### 色を変える
→ `:root { }` の CSS変数を変更（上の表を参照）

### 食事カードを追加・変更
→ `#food .food-grid` 内の `.food-card` を追加・編集

### プランを変更
→ `#plan-1` / `#plan-2` / `#plan-3` のHTML部分  
　 ＋ `<script>` 内の `PLAN_DATA` オブジェクト（費用・アドバイス）の両方を更新

### 旅行日程を変更
→ ヒーロー `.eyebrow` テキスト、`.hero-info span`、`<title>`、`footer p` を変更

---

## 環境情報

- **プレビューサーバー**: `http://localhost:8765/choshi-travel.html`  
  （`~/Downloads/.claude/launch.json` で python3 HTTPサーバー設定済み）  
  更新後は `cp index.html /tmp/choshi-travel.html` でプレビューに反映
- **ブランチ**: `main` のみ（ブランチ運用なし）
- **直近の変更**: 2026-05-29 白ベース・ガイドブック風リデザイン・GitHub Pages 公開
