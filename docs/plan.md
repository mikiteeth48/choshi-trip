# 銚子旅行プロジェクト — 実装記録

> 設計の詳細・デプロイ手順などは **CLAUDE.md**（プロジェクトルート）を参照。
> このファイルは「何をいつ作ったか」の履歴メモです。

---

## 変更履歴

### 2026-05-29 — GitHub Pages 公開 + プロジェクト整備
- `choshi-travel.html` → `choshi-trip/index.html` に移行
- GitHub リポジトリ作成（mikiteeth48/choshi-trip）
- GitHub Pages 有効化 → https://mikiteeth48.github.io/choshi-trip/
- `CLAUDE.md` 作成（Claude の記憶ファイル）
- `.gitignore` / `.env` 設定（トークン管理）

### 2026-05-29 — 白ベース・ガイドブック風リデザイン
- ヒーローセクション以降を白/クリームベースに全面変更
- 食事カードに写真追加（銚子の景観写真を文脈的に配置）
- 食事カードに実店舗リンク追加
  - 銚子丸 / 銚子電鉄 / ヤマサ醤油 / ヒゲタ醤油 / 銚子市観光協会

### 2026-05-28 — 機能実装（6フェーズ）
| フェーズ | 内容 |
|---------|------|
| Phase 1 | レスポンシブCSS（768px / 480px） |
| Phase 2 | 時刻表リンク（銚子電鉄・Yahoo!乗換） |
| Phase 3 | Google Maps リンク（📍 タイムライン） |
| Phase 4 | 写真ライトボックス（グループナビ付き） |
| Phase 5 | 詳細モーダル（費用・アドバイス・交通リンク） |
| Phase 6 | スティッキーナビゲーション |

### 2026-05-28 — 初版コンテンツ制作
- 3プランのタイムライン（のんびり派・探検派・食いしん坊派）
- 食事・お土産ガイド（6カテゴリ）
- 見どころセクション（6スポット）
- トレインバー（往路・帰路・注意）
- ヒーロービジュアル（犬吠埼の日の出写真）

---

## 技術メモ

### スクロールロック（モーダル / ライトボックス共用）
```javascript
let _lockCount = 0;
const lockScroll   = () => { if (!_lockCount++) document.body.style.overflow = 'hidden'; };
const unlockScroll = () => { if (_lockCount > 0 && !--_lockCount) document.body.style.overflow = ''; };
```
カウンタ方式で両方から呼べる設計。

### plan-photo の pointer-events
```css
.plan-photo-overlay { pointer-events: none; }
```
オーバーレイがある場合も写真クリックを拾えるよう必須設定。

### モバイルでの plan-photo 消失バグ対策
```css
@media (max-width: 768px) {
  .plan-photo { min-height: 280px; }  /* position:absolute の img が消えないよう */
}
```

### ESCキー競合（ライトボックス vs モーダル）
ライトボックスが開いている場合はモーダルの ESC ハンドラをスキップ：
```javascript
if (document.getElementById('lightbox').classList.contains('lb-open')) return;
```
