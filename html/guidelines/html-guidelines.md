# HTMLコーディングガイドライン

![WIP](https://img.shields.io/badge/status-WIP-yellow)

このドキュメントは、HTMLの記述ルールを整理し、プロジェクト間でのブレをなくすためのリファレンスです。SCSSガイドラインとセットで使用し、デザインシステムの基盤となるマークアップルールを定義します。

---

## 1. 基本方針

- HTMLは**セマンティック**に記述します。
- **再利用性**と**アクセシビリティ**を優先します。
- SCSSのBEM命名規則と整合性をとります。
- JavaScriptとの連携やテストを見据えた構造にします。
- コンポーネントの構造は「primitive（基本UI部品）」と「semantic（文脈を持つ構造）」で分けて設計します。
  - primitive: 小さな部品（例：button, input, icon など）
  - semantic: 意味や役割を持つUI構成（例：card, header, tabs など）
  - この分類により、マークアップの一貫性・再利用性を高めることを目的とします。SCSSの構造やコンポーネントディレクトリとも整合性を取ることが重要です。

---

## 2. DOCTYPEと基本構造

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

    <title>ページタイトル | サイト名</title>
    
    <meta name="description" content="このページの内容を簡潔に説明。120文字以内推奨。">
    <meta name="keywords" content="キーワード1, キーワード2, キーワード3">
    <meta name="author" content="運営会社・担当者名">
    <meta name="robots" content="index, follow">
    <link rel="canonical" href="https://example.com/page/">

    <meta property="og:type" content="website">
    <meta property="og:title" content="ページタイトル | サイト名">
    <meta property="og:description" content="SNSシェア用のページ説明文。SEO descriptionと近い内容でOK。">
    <meta property="og:url" content="https://example.com/">
    <meta property="og:site_name" content="サイト名">
    <meta property="og:image" content="https://example.com/assets/ogp.jpg">
    <meta property="og:locale" content="ja_JP">

    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:site" content="@アカウント名">
    <meta name="twitter:title" content="ページタイトル | サイト名">
    <meta name="twitter:description" content="Twitterシェア用ページ説明。">
    <meta name="twitter:image" content="https://example.com/assets/ogp.jpg">

    <link rel="icon" href="/favicon.ico">
    <link rel="apple-touch-icon" href="/apple-touch-icon.png">
    <link rel="manifest" href="/site.webmanifest">

    <link rel="stylesheet" href="/styles/style.css">

    <script type="application/ld+json">
      {
        "@context": "https://schema.org",
        "@type": "Organization",
        "name": "会社名",
        "url": "https://example.com/",
        "logo": "https://example.com/assets/logo.png",
        "sameAs": [
          "https://www.facebook.com/yourcompany",
          "https://twitter.com/yourcompany",
          "https://www.instagram.com/yourcompany/"
        ]
      }
    </script>
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'G-XXXXXXXXXX');
    </script>
  </head>

  <body class="theme--light">
    <div class="app">
      <div class="app__container">
        <header class="header"></header>
        <main id="main-content" class="main"></main>
        <aside class="aside" aria-label="補足情報"></aside>
        <footer class="footer"></footer>
        
        <div class="fixedButtons" aria-hidden="true"></div>
        <div class="mask" aria-hidden="true"></div>

        <div class="drawer" role="dialog" aria-hidden="true" aria-modal="true"></div>
        <div class="dialog" role="dialog" aria-hidden="true" aria-modal="true"></div>
        <div class="alert" role="alert" hidden></div>
      </div>
    </div>
    <script src="" defer></script>
  </body>
</html>
```

[htmlファイルはこちら](../resources/html/template.html)

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

## 7. コメントの書き方（開発メモ・設計意図の共有）

開発用のメモは `<!-- TODO: -->` のように明示的に記述します。
以下に、よく使われるタグとその用途をまとめます。コードの近くに簡潔に記述することで、保守性・可読性を高めることができます。

### コメントタグ一覧

| タグ           | 用途                           | 内容例                                                   |
|----------------|--------------------------------|----------------------------------------------------------|
| `TODO:`        | 実装予定                       | まだ作っていない部分・後回しにしている実装               |
| `FIXME:`       | 修正が必要                     | 現状バグっている・非対応になっている箇所                 |
| `NOTE:`        | 補足情報・設計意図             | 意図的にこうしている・例外対応の理由                     |
| `HACK:`        | やむを得ない仮対応             | 理想ではないが、暫定的に対応している                     |
| `REVIEW:`      | レビュー対象                   | 設計やスタイルが不安な箇所（他の人に意見をもらいたい）   |
| `OPTIMIZE:`    | パフォーマンス改善余地         | 効率の悪い処理・無駄な指定があるかもしれない箇所         |
| `DEPRECATED:`  | 廃止予定のスタイル             | 将来的に削除・変更される見込みのあるスタイル             |
| `BUG:`         | 明確なバグ                     | 実際にバグ報告があった・不具合が再現している              |
| `IMPORTANT:`   | 変更に注意が必要な重要箇所     | 依存関係が多く、変更が広範囲に影響する箇所               |
| `WARNING:`     | 副作用・注意が必要なスタイル   | デバイスやブラウザによって挙動が変わる等                 |

### コメント記述の例（html）

```html
<!-- TODO: ヘッダーのSPレイアウトに対応する -->
<!-- FIXME: iOS Safariでpaddingが無視されるバグあり -->
<!-- NOTE: このマージンはデザイン確認用に一時的に大きめに設定 -->
<!-- HACK: gridでは崩れるためflexで強引に揃えている -->
<!-- REVIEW: justify-contentをspace-betweenにしてよいか再確認 -->
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

### コンポーネント内でヘッダー、フッターがあるもの（例：カード）

```html
<article class="card">
  <div class="card__container">
    <div class="card__thumbnail">
      <img src="/img/sample.jpg" alt="..." class="image__img" />
    </div>
    <div class="card__main">
      <header class="card__header">
        <h2 class="card__title">サービス名</h2>
      </header>
      <div class="card__content">
        <p>説明文...</p>
      </div>
      <footer class="card__footer">
        <a href="#" class="button button--primary">詳しく見る</a>
      </footer>
    </div>
  </div>
</article>
```

### card__content 内でさらに2段組にしたい場合

```html
<article class="card">
  <div class="card__container">
    <div class="card__thumbnail">
      <img src="/img/sample.jpg" alt="..." class="card__thumbnail__img" />
    </div>
    <div class="card__main">
      <header class="card__header">
        <h2 class="card__title">サービス名</h2>
      </header>
      <div class="card__content">
        <div class="card__content__main"></div>
        <div class="card__content__aside"></div>
      </div>
      <footer class="card__footer">
        <a href="#" class="button button--primary">詳しく見る</a>
      </footer>
    </div>
  </div>
</article>
```

#### 2カラム・複数領域を作るときの命名ルール（カード内など）

- 意味があるとき：`__header`, `__main`, `__aside`, `__meta`
  - `__header`：領域内の導入部分・見出し要素に使用（`<h3>`やラベルなど）
  - `__main`：そのスコープでの主たる情報
  - `__aside`：補足的情報（画像、タグ、時刻など）
  - `__meta`：属性的な情報（ステータス、ラベル、作成者など）

### 埋め込む系のコンポーネントの場合

```html
<div class="xxx">
  <div class="xxx__container">
    <div class="xxx__embed">
      ここに埋め込みコンテンツが入ります。
    </div>
  </div>
</div>
```

### 普通のコンポーネントの場合

```html
<div class="xxx">
  <div class="xxx__container">
    <div class="xxx__content">
      ここにコンテンツが入ります。
    </div>
  </div>
</div>
```

### 例外 ： Headless UI 関連（例：checkbox）
```html
<div class="checkbox-field">
  <div class="checkbox-field__container">
    <div class="checkbox">
      <div class="checkbox__box">
        <svg class="checkbox__icon"></svg>
      </div>
    </div>
    <label class="checkbox-field__label">ブラウザに保存する</label>
  </div>
</div>
```


---

## 10. 補足

- JSX/MDXとの統合も想定する場合、自己終了タグ（例：`<input />`）の統一も検討。
- WordPress・Next.jsなど環境ごとの制限がある場合は補足ルールを記載。