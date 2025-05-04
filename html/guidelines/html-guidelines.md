# HTMLコーディングガイドライン

このドキュメントは、HTMLの記述ルールを整理し、プロジェクト間でのブレをなくすためのリファレンスです。SCSSガイドラインとセットで使用し、デザインシステムの基盤となるマークアップルールを定義します。

---

## 1. 基本方針

- HTMLは**セマンティック**に記述します。
- **再利用性**と**アクセシビリティ**を優先します。
- SCSSのBEM命名規則と整合性をとります。
- JavaScriptとの連携やテストを見据えた構造にします。

---

## 2. DOCTYPEと基本構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ページタイトル</title>
</head>
<body class="theme--light">
  <div class="app">
    <header class="header">...</header>
    <main class="main">...</main>
    <footer class="footer">...</footer>
  </div>
</body>
</html>
```

---

## 3. セマンティックなタグの使用

| 機能 | 使用タグ（推奨） |
|------|------------------|
| ヘッダー | `<header>` |
| メインコンテンツ | `<main>` |
| フッター | `<footer>` |
| セクション | `<section>` |
| 記事単位 | `<article>` |
| ナビゲーション | `<nav>` |
| 補足・注意情報 | `<aside>` |
| 見出し | `<h1>`〜`<h6>`（階層順） |

- `div` は構造上必要なときのみに使用。
- ページごとに `<h1>` は **1つのみ**。
- セクション内に見出しを必ず入れる。

---

## 4. class 命名規則（SCSSとの連携）

SCSSの命名と一致させます（BEMベース）

```html
<div class="button button--primary button--m">送信</div>
<div class="card__header">...</div>
<ul class="list list--A01">
  <li class="list__item">...</li>
</ul>
```

- modifier（状態クラス）は半角スペースで分離：`button --active`
- `u--` で始まるユーティリティクラスは例外的な装飾用に限定

---

## 5. 属性の記述ルール

- 属性は以下の順序を推奨：

```
id → class → data-* → aria-* → role → name → type → value → required → disabled → tabindex → href → src → alt → title → style
```

- Boolean属性（例：`disabled`）は **値をつけない**：

```html
<input type="checkbox" disabled>
```

- `data-*` 属性は構造に関係しない情報伝達のみに使用。

---

## 6. アクセシビリティ対応

- `img` には必ず `alt` 属性を指定。
- `form`要素には`label`を関連付ける。
- `aria-*` 属性は、JSコンポーネントやインタラクション要素に明示的に付与。
- カスタムUIには `role` を指定：

```html
<div role="button" tabindex="0" aria-pressed="false">...</div>
```

---

## 7. コメントの書き方

開発用のメモは `<!-- TODO: -->` のように明示的に記述。

```html
<!-- TODO: このエリアはAPI連携後に表示される -->
<!-- NOTE: セクションごとに見出しが必要 -->
```

---

## 8. 禁止事項

- `style` 属性でのインラインスタイル指定（開発時の一時使用を除く）
- `id` セレクタの多用（スタイル指定には使わない）
- `<font>`, `<center>`, `<b>`, `<i>` などの非推奨タグ
- 複数の`<h1>`使用

---

## 9. よくある構造例（コンポーネント）

### ボタン

```html
<button class="button button--primary button--l" type="submit">
  送信する
</button>
```

### 入力フォーム

```html
<label for="email" class="form__label">メールアドレス</label>
<input id="email" type="email" class="form__input" required>
```

### カード

```html
<article class="card">
  <div class="card__header">...</div>
  <div class="card__body">...</div>
  <footer class="card__footer">...</footer>
</article>
```

---

## 10. 補足

- JSX/MDXとの統合も想定する場合、自己終了タグ（例：`<input />`）の統一も検討。
- WordPress・Next.jsなど環境ごとの制限がある場合は補足ルールを記載。