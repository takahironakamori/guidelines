# ファビコン画像とogp画像の作成方法と設置方法

## 作る必要がある画像

最低限必要な画像は、以下の4つです。

- `ogp.png` w:1200px, h:630px
- `favicon.ico` w:32px, h:32px
- `icon.svg` w:512px, h:512px
- `apple-touch-icon.png` w:180px, h:180px

さらにPWA (プログレッシブウェブアプリ) 化させる場合は、以下も作成します。

- `icon-192x192.png` w:192px, h:192px
- `icon-512x512.png` w:512px, h:512px
- `icon-maskable-512x512.png` w:512px h:512px

## 設置方法

```html
<meta property="og:image" content="/ogp.png">

<meta name="twitter:image" content="/ogp.png">

<link rel="icon" href="/favicon.ico" sizes="32x32">
<link rel="icon" href="/icon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
```

PWA (プログレッシブウェブアプリ) 化用の画像は、マニフェストファイルを作成し、設置します。

```html
<link rel="manifest" href="/site.webmanifest">
```

`site.webmanifest` 内に以下のようにアイコンの情報を記述します。

```json
{
  /* 省略 */
  "icons": [
    {
      "src": "/assets/images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/assets/images/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    },
    {
      "src": "/assets/images/icons/icon-maskable-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "maskable"
    }
  ]
}
```

設置場所はルートから指定すればどこでも大丈夫です。

## 作成方法

[画像作成用XDファイルはこちら](../resources/icon-template.xd)

### opg画像

個別で作成します。

### アイコン画像

- `icon.svg` を作成します。  w:409px, h:409px の枠内に入るように作ります。
- `icon.svg` をもとに、`icon-maskable-512x512.png` を作ります。`icon-maskable-512x512.png` は `icon.svg` を拡大したバージョンと考えて差し支えありません。
- `icon.svg` をもとに、`icon-192x192.png`(w:192px, h:192px), `icon-512x512.png`(w:512px h:512px)を書き出す。
- [Favion ジェネレーター](https://favicon-generator.mintsu-dev.com/) で、`icon-192x192.png` を使って `favicon.ico`(w:32px, h:32px) を作成します。

