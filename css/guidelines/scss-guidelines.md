# SCSSコーディングガイドライン

![WIP](https://img.shields.io/badge/status-WIP-yellow)

このドキュメントは、私自身のSCSSコーディングルールを整理し、プロジェクト間でのブレをなくすためのリファレンスとして作成したものです。

---

## 命名規則（BEM）

### 基本構造

```
.prefix--block {}
.prefix--block--variation {}
.prefix--block__element {}
.prefix--block--variation__element {}
.prefix--block__element--variation {}
.prefix--block--variation__element--variation {}
```

### prefix（接頭辞）

- プロジェクト名、ページの種類、役割等、ざっくりとした役割を明示するために prefix（接頭辞）をつけることができます。
- 公開用のウェブページ等、prefix をつけないこともできます。

#### 例

- 管理画面のタブ : `admin--tabs`
- エディタのタブ（管理画面のタブとはわけておきたい） : `blog-editor--tabs`
- 公開用ホーム画面のヘッダ（prefix無し） : `header`

### block(かたまり)

- コンポーネント名を block として使用できます。
- 複数の単語でコンポーネント名が構成される場合は、`-` でつなぎます。 

#### 例

- 管理画面のタブ : `admin--tabs`
- 公開用ホームページのお問い合わせフォーム : `contact-form`

#### 「複数」の block の命名規則

| 正しい | 使用不可の例 | 
|-------|-------|
| `--buttons` | `--button-group` |
| `--cards` | `--card-group` |
| `--tabs` | `--tab-group` |
| `--items` | `--list` |


### variation(バリエーション)

- block や element について、バリエーションがある場合は、variation をつけます。
- variation は `A01` のような汎用なものや、`primary` などのラベルをつけることができます。
- variation は block 、element のそれぞれについて複数回使用することはできません。
- 下記の modifiter で使用している状態名を使用することはできません。

#### 例

- 管理画面のタブの別バージョン : `admin--tab--A01`
- お問い合わせフォームの重要なボタン : `contact-form__button--primary`


#### pattern : パターンバリエーション

- 特定の意味を持たせず、デザインの違いだけを区別するためのバリエーションは `--A01`, `--A02` のように命名します。このような記号的バリエーションを **パターンバリエーション** と呼びます。
- 明確な意味を持つ場合は --primary などのラベル付きバリエーションを使います。

```scss
.button--A01 {} // デザインパターン1
.button--A02 {} // デザインパターン2
```


#### style : スタイルバリエーション

- 見た目を変更するバリエーション
- スタイルバリエーションは、命名が長くなりすぎないように特定の表記については **省略表記（略称）を使って統一**します(下記、「省略表記していいもの」、「省略表記できないもの」参照)。

```scss
.button--fill {} // 塗りつぶしのボタン
.button--outline {} // 枠線だけのボタン
```


#### size : サイズバリエーション

- 大きさを指定するバリエーション
- ボタンや要素のサイズバリエーションは、命名が長くなりすぎないように **省略表記（略称）を使って統一**します(下記、「省略表記していいもの」、「省略表記できないもの」参照)。

```scss
.button--s {}
.button--m {}
.button--l {}
.button--xl {}
.button--xxl {}
```

#### intent : インテントバリエーション

- 特定の意味を示す名称を持つバリエーションで、以下があります。

| ラベルバリエーション | 推奨用途                            | イメージ                         | 対象アクションの例                     |
|----------------------|-------------------------------------|----------------------------------|----------------------------------------|
| `--primary`          | 最重要アクション・ページで1つだけ  | ブランドカラー or 強調されたボタン | フォームの送信 / 次へ進む / 購入する     |
| `--secondary`        | 補助的なアクション（primaryの次に大事） | 落ち着いたカラー / 控えめ         | 戻る / キャンセル / 編集する           |
| `--info`             | 情報の提示や通知                    | 青系カラーで冷静・中立           | 詳細を見る / お知らせ / チュートリアル開始 |
| `--positive`         | 成功・完了など良い結果を強調        | 緑系 / 明るい安心感              | 保存成功 / 完了 / 有効化する            |
| `--notice`           | 注意喚起・軽い警告                  | オレンジ・黄色系で目を引く       | 一部未入力 / 古いデータの確認 / 下書き保存 |
| `--negative`         | 否定・削除・注意の必要なアクション   | 赤系 / 危険・緊急を連想          | 削除 / 退会 / キャンセル確認            |
| `--accent`           | 装飾的・強調目的（機能的ではない）   | 補助カラー / アクセント的使い方   | フィルタータグ / ラベル表示 / 特別扱い要素 |

- ユーザーの行動を誘導する強さ順に： primary > secondary > info / positive<br>
フィードバック的要素： positive, notice, negative<br>
視覚の補助要素： accent

#### バリエーションが複数ある場合

- クラス名は、以下の順番で variation をつけて記述します。
- バリエーションが複数ある場合は、バリエーションごとにクラスを分離して定義・使用する方式を原則とします。
- 順番：`block` `block--[pattern]` `block--[style]` `block--[size]` `block--[intent]`

#### 使用例

| バリエーション | 正しい使用例 | 誤りの使用例 |
|--------------------|--------------------|-----------------------|
| `A01`, `fill`, `m`, `primary` | `button` `button--A01` `button--fill` `button--m` `button--primary` | `button--A01--fill--m--primary` |


### element(構成要素)

- コンポーネントを構成する要素を element と使用できます。
- element は入れ子にすることができます。
- element のネスト階層は2階層以内（`__`を使うのは2回まで）が理想だが、明確な意味があり、読みやすさ・保守性に問題がなければ3階層以上も許容します。

#### 例

- 管理画面のタブのラベル : `admin-tab__label`
- ヘッダのロゴ : `header__logo`
- ヘッダーのロゴの画像 : `header__logo__img`


### modifier(状態)

- modifier は、以下に明記されたもののみ使用することができます。
- modifier は、必ず "--" で始まります。
- modifier は必ず別のクラスとして使用します（半角スペースで区切ってください）。
- modifier のスタイルは、それぞれのコンポーネントで定義します。
- 同時に複数の modifier を使用することができます。

#### 選択されている状態

| 正しい | 使用不可の例 | 
|-------|-------|
| `--active` | `--is-active`, `--selected`, `--checked` |

#### エラーがある状態

| 正しい | 使用不可の例 | 
|-------|-------|
| `--error` | `--is-error`, `--has-error` |

#### 開いている状態

| 正しい | 使用不可の例 | 
|-------|-------|
| `--open` | `--is-open`, `--opened` |

#### アニメーションで表示する

- 【正】まず `--on` を加え、すぐ（10ms）に `--show` を加える。
- 【誤】`--show` だけを加える。
- 【誤】`--visible` を使用する。

#### アニメーションで非表示する

- 【正】あらかじめ `--on --show` をつけておき、まず `--show` を消して、数秒（250ms）後に `--on` を消す。
- 【誤】まず `--hide` を加え、数秒後に `--off` を加える。

#### 使用不可

| 正しい | 使用不可の例 | 
|-------|-------|
| `--disabled` | `--is-disabled`, `--disable`, `--none` |

#### 非表示

| 正しい | 使用不可の例 | 
|-------|-------|
| `--hidden` | `--is-hidden`, `--invisible`, `--hide` |

#### ローディング中

| 正しい | 使用不可の例 | 
|-------|-------|
| `--loading` | `--is-loading`, `--load`, `--spinner` |

#### 操作不能

| 正しい | 使用不可の例 | 
|-------|-------|
| `--readonly` | `--read-only`, `--no-edit` |

#### フォーカス中

| 正しい | 使用不可の例 | 
|-------|-------|
| `--focus` | `--focused`, `--is-focus` |

#### ホバー中

| 正しい | 使用不可の例 | 
|-------|-------|
| `--hover` | `--hovered`, `--is-hover` |

---

### utility(例外処理)

- utility を使用して例外的に表示を変更することができます。
- utility は、必ず "u--" で始まり、基本構造は u--block--variation です。
- utility は必ず別のクラスとして使用します（半角スペースで区切ってください）。
- 同時に複数の utility を使用することができます。

#### 例

- 段落の余白（下）を大きくする : `paragraph--m n--mb--l`

## SCSS構造とファイル分割

```
/ styles
  ├── style.scss                ... ウェブページが読み込むファイル。app 以下必要なSCSSを読み込む。
  ├── app                       ... ページ別の scss。 値は themes ディレクトリ内で指定しているものしか使えない。
  │   ├── app.scss              ... アプリ全体で使用する scss。
  │   └── [page_name].scss      ... ページ個別で使用する scss（例:home, category, single, login, page）。
  ├── components                ... コンポーネント別の scss。値は themes ディレクトリ内で指定しているものしか使えない。
  │   ├── designToken           ... Design Token別の scss（Theme に該当する部分）。
  │   │   ├── designToken.scss  ... Primitive Component が読み込むファイル。Design Token内の scss を束ねる。
  │   │   ├── border.scss       ... ボーターに関する定義。
  │   │   ├── color.scss        ... 色に関する定義。
  │   │   ├── icon.scss         ... アイコンに関する定義。
  │   │   ├── layout.scss       ... レイアウトに関する定義。
  │   │   ├── space.scss        ... スペースに関する定義。
  │   │   ├── transition.scss   ... アニメーションに関する定義。
  │   │   ├── typography.scss   ... テキストに関する定義。
  │   │   ├── utility.scss      ... utility を定義。
  │   │   └── common.scss       ... 上記の scss ファイルに含まれてないものをまとめて定義する。
  │   ├── primitive             ... Primitive Component別の scss。
  │   │   └── [component_name].scss ... Primitive Component の scss（例:paragraph）。
  │   ├── semantic              ... Semantic Component別の scss。
  │   │   └── [component_name].scss ... Semantic Component の scss（例:header）。
  └── tokens                    ... cssの値を指定している scss。
      ├── schemes               ... Design Tokenと接続するための scss。
      │   ├── _schemes.scss     ... designToken内のファイル が読み込むファイル。_common,_dark,_lightを束ねる。
      │   ├── _common.scss      ... 共通部分の scss。
      │   ├── _dark.scss        ... ダークカラーテーマ用 scss。
      │   └── _light.scss       ... 標準カラーテーマ用 scss。
      ├── aliases
      │   ├── _common.scss      ... values/color 以外の values 内の scss を束ねる。
      │   ├── _dark.scss        ... ダークカラーテーマ用の色に関する scss を束ねる。  
      │   └── _light.scss       ... 標準カラーテーマ用の色に関する scss を束ねる。  
      └── values
          ├── color             ... 色に関する値を指定する。
          │   ├── _dark.scss    ... ダークカラーテーマ用の色の値を指定する scss。 
          │   ├── _light.scss   ... 標準カラーテーマ用の色の値を指定する scss。   
          │   └── _static.scss  ... 固定カラーテーマ用の色の値を指定する scss。  
          ├── _background.scss  ... background に関する値を指定する scss。
          ├── _border.scss      ... border に関する値を指定する scss。
          ├── _box-shadow.scss  ... box-shadow に関する値を指定する scss。   
          ├── _box-sizing.scss  ... box-sizing に関する値を指定する scss。
          ├── _clear.scss       ... clear に関する値を指定する scss。
          ├── _cursor.scss      ... cursor に関する値を指定する scss。    
          ├── _display.scss     ... display に関する値を指定する scss。
          ├── _float.scss       ... float に関する値を指定する scss。
          ├── _font.scss        ... font に関する値を指定する scss。   
          ├── _image.scss       ... image に関する値を指定する scss。
          ├── _list.scss        ... list に関する値を指定する scss。
          ├── _margin.scss      ... margin に関する値を指定する scss。    
          ├── _overflow.scss    ... overflow に関する値を指定する scss。
          ├── _padding.scss     ... padding に関する値を指定する scss。
          ├── _position.scss    ... position に関する値を指定する scss。   
          ├── _table.scss       ... table に関する値を指定する scss。
          ├── _text.scss        ... text に関する値を指定する scss。
          ├── _transition.scss  ... transition に関する値を指定する scss。 
          ├── _visibility.scss  ... visibility に関する値を指定する scss。 
          └── _z-index.scss     ... z-index に関する値を指定する scss。    
```

---

## スタイルの設定

- app, components ディレクトリ内の各 scss ファイルでのスタイルは、以下のように変数で設定します。
- 色に関する変数は themes/_dark.scss, _light.scss の両方で指定されている変数名を指定します。
- 色以外に関する変数は themes/common.scss で指定されている変数名を指定します。

### 誤っている例（値を指定している）
```
.page {
  margin: 0;
  background: rgb(255, 255, 255);
}
```

### 正しい例（変数名を指定している）
```
.page {
  margin: var(--margin--none);
  background: var(--background-color--default);
}
```

---

## スタイルのプロパティの書き順

プロパティを以下のような大きなカテゴリで分けて、基本この順番で書きます。

### 1.　レイアウト・表示設定

- `display`
- `visibility`
- `position`
- `top`, `right`, `bottom`, `left`, `inset`
- `z-index`

### 2.フレックス・グリッド系

- `flex`
- `flex-grow`, `flex-shrink`, `flex-basic`
- `justify-content`
- `align-items`
- `align-self`
- `grid-template-columns`
- `grid-template-rows`
- `gap`
- `row-gap`, `column-gap`

### 3.サイズ・ボックスモデル

- `width`
- `min-width`, `max-width`
- `height`
- `min-height`, `max-height`
- `margin`
- `padding`
- `box-sizing`
- `overflow`, `overflow-x`, `overflow-y`

### 4.ボーダー・背景・シャドウ

- `border`
- `border-width`, `border-style`, `border-color`
- `border-radius`
- `background`
- `background-color`
- `background-image`
- `box-shadow`

### 5.テキスト・フォント設定

- `font`
- `font-size`
- `font-weight`
- `font-family`
- `line-height`
- `letter-spacing`
- `color`
- `text-align`
- `text-decoration`
- `text-transform`

### 6.その他の視覚効果

- `cursor`
- `opacity`
- `user-select`
- `outline`
- `transform`
- `transition`
- `animation`

### 例
```css
* {
  // Layout
  display: flex;
  position: relative;
  top: 0;
  left: 0;
  z-index: 1;

  // Flex/Grid
  justify-content: center;
  align-items: center;

  // Size/Box Model
  width: 100%;
  margin: 0 auto;
  padding: 16px;

  // Border/Background/Shadow
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

  // Typography
  font-size: 1rem;
  font-weight: bold;
  line-height: 1.0;
  color: #333;
  text-align: center;

  // Visual Effects
  cursor: pointer;
  opacity: 1;
  user-select: none;
  outline: none;
  transform: translateY(0);
  transition: all 0.3s ease;
}
```
---

## SCSS変数の命名規則

Design Token を定義するための SCSS 変数は、CSS変数と対応する形で命名します。

### 命名構造
$[component]--[variation]--[state]--[property]

#### 例
$button--primary--focus--background-color: #0056b3;

→ CSSでは以下のように出力される：
--button--primary--focus--background-color: #{$button--primary--focus--background-color};

### 命名の優先順
- コンポーネント名（button, form, etc）
- バリエーション（primary, secondary, etc）
- 状態（hover, focus, active, disabled）
- プロパティ（background-color, border-color, opacity など）

### 注意
- 意味と状態の順序は逆にしない（❌ `$button--background-color--focus`）
- 省略語は CSS と同様に統一されたものだけを使う（例：`m`, `l`, `xl`など）

---

## 変数生成ルール（Design Token 自動生成用）

このプロジェクトでは、変数命名ルールが体系化されているため、ChatGPTなどのAIツールを使って以下のような依頼を行うことで、機械的にCSS変数やSCSSスタイルを自動生成できます。

### 🔠 命名構造

```
--[component]--[pattern]--[style]--[intent]--[state]--[property]
```

- `component`（必須）  
  対象となるUIコンポーネント名（例：`button`, `checkbox`）

- `pattern`（任意）  
  特定の意味を持たせず、デザインの違いだけを区別するためのバリエーション（例：`A01`, `B04`）

- `style`（任意）  
  コンポーネントの見た目スタイル（例：`fill`, `outline`, `ghost`）

- `intent`（任意）  
  ボタンの役割や文脈を表すセマンティックなラベル（例：`primary`, `secondary`, `positive`, `notice`, `accent`）

- `state`（任意）  
  UIの状態（例：`hover`, `focus`, `active`, `disabled`）

- `property`（必須）  
  スタイルのプロパティ名（例：`color`, `background-color`, `border-color`, `opacity`）

> 任意項目は存在しない場合、省略されますが、順序は崩さずに詰めること。

---

### 🧾 命名例

| 状況 | 変数名例 |
|------|----------|
| 通常の primary ボタンの背景色 | `--button--primary--background-color` |
| outline スタイルの hover 状態の border 色 | `--button--outline--hover--border-color` |
| positive intent + active 状態の color | `--button--positive--active--color` |
| style + intent + 状態がすべてある場合 | `--button--outline--secondary--focus--background-color` |

---

### 🤖 自動生成用途での使用例（ChatGPTなど）

以下のように依頼すれば、SCSS構文付きで自動生成できます：

```text
button コンポーネントの
outline スタイルにおいて、
intent は primary, secondary、
state は hover, focus, active、
プロパティは color, background-color, border-color。
それぞれの組み合わせで、

1. CSS変数（.theme--light / .theme--dark に分けて）
2. SCSSのスタイル定義（.button--outline--[intent]）

を出力してください。
```

生成例：

1. CSS変数定義（テーマ別）

```scss
.theme--light {
  --button--outline--primary--hover--color: var(--color--blue-700);
  ...
}

.theme--dark {
  --button--outline--primary--hover--color: var(--color--blue-400);
  ...
}
```

2. SCSSスタイル定義

```scss
.button--outline--primary {
  &:hover {
    color: var(--button--outline--primary--hover--color);
    ...
  }
  &:focus { ... }
  &:active { ... }
}
```


---

## 省略表記していいもの

### 位置

| 位置 | 推奨略称（正しい） | 使用不可の例（誤り） |
|--------|--------------------|-----------------------|
| all(top, bottom, left, right) | `a` | `all`        |
| vertical                      | `v` | `vertical`   |
| horizontal                    | `h` | `horizontal` |


### サイズ

| サイズ | 推奨略称（正しい） | 使用不可の例（誤り） |
|--------|--------------------|-----------------------|
| extra-extra-small | `xxs` | `extra-extra-small` |
| extra-small       | `xs`  | `extra-small`       |
| small             | `s`   | `small`             |
| medium            | `m`   | `medium`            |
| large             | `l`   | `large`             |
| extra-large       | `xl`  | `extra-large`       |
| extra-extra-large | `xxl` | `extra-extra-large` |


### htmlの仕様で省略されているもの

| サイズ | 推奨略称（正しい） | 使用不可の例（誤り） |
|--------|--------------------|-----------------------|
| column | `col` | `column` |


## 省略表記できないものの例

### 位置

| 位置 | 推奨略称（正しい） | 使用不可の例（誤り） |
|--------|--------------------|-----------------------|
| top    | `top`    | `t` |
| bottom | `bottom` | `b` |
| left   | `left`   | `l` |
| right  | `right`  | `r` |

### ウェイト

| ウェイト | 推奨名称（正しい） | 使用不可の例（誤り） |
|--------|--------------------|-----------------------|
| thin        | `thin`        | `t`  |
| extra-light | `extra-light` | `el` |
| light       | `light`       | `l`  |
| regular     | `regular`     | `r`  |
| medium      | `medium`      | `m`  |
| semi-bold   | `semi-bold`   | `sb` |
| bold        | `bold`        | `b`  |
| extra-bold  | `extra-bold`  | `eb` |
| black       | `black`       | `b`  |

---

## 禁止事項

- `!important` の多用
- `id`セレクタでのスタイリング
- JavaScriptの状態クラスと整合しない命名

---

## メンテナンス時のチェックポイント

- クラス名に意味があるか？
- コンポーネントが再利用しやすくなっているか？
- スタイルがコンポーネント単位で閉じているか？
- 古い変数や未使用スタイルが混在していないか？
- tokens 内以外の scss ファイルで値を直接指定していないか？

---

## コメントタグの使い方（開発メモ・設計意図の共有）

開発中の意図や後で見返したときの理解を助けるために、SCSSファイル内に「コメントタグ」を活用できます。  
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

### コメント記述の例（SCSS）

```scss
// TODO: ヘッダーのSPレイアウトに対応する
// FIXME: iOS Safariでpaddingが無視されるバグあり
// NOTE: このマージンはデザイン確認用に一時的に大きめに設定
// HACK: gridでは崩れるためflexで強引に揃えている
// REVIEW: justify-contentをspace-betweenにしてよいか再確認
```