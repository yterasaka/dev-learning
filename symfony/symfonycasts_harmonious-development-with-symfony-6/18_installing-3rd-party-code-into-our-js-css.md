# 18. Installing 3rd Party Code into our JS & CSS

## 学習日

- 2024/11/01

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#18 Installing 3rd Party Code into out JS & CSS](https://symfonycasts.com/screencast/symfony6/js-vendor-libs#play)

## 3rd パーティのライブラリをインストールする

- 以下の手順で、3rd パーティのライブラリをインストールする。
- 例: bootstrap

```bash
yarn add bootstrap --dev
# or
npm add bootstrap --include=dev
```

- `app.css` に移動して、`@import` で bootstrap を読み込む。

```css
@import "~bootstrap";
```

## `node_modules/` から特定のファイルを読み込む

- `@import` で読み込んでいるファイルがどこにあるかを確認するには、`command` キーを押しながらファイル名をクリックする。
- 以下のように、`node_modules/` から必要なファイルを読み込むことができる。

```css
@import "~bootstrap";
@import "~@fortawesome/fontawesome-free/css/all.css";
@import "~@fontsource/roboto-condensed";
```
