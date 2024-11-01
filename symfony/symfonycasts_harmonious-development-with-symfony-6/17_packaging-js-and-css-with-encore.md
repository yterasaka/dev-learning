# 17. Packaging JS and CSS with Encore

## 学習日

- 2024/11/01

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#17 Packaging JS and CSS with Encore](https://symfonycasts.com/screencast/symfony6/webpack-encore)

## Webpack Encore

- Webpac Encore をインストールした時に、`assets/` ディレクトリが作成される。
- Encore は、JavaScript や CSS、画像などのアセットをまとめて、一つのファイルにビルドする機能を提供する。
- これにより、アセットの読み込みが高速化される。

## `webpack.config.js` ファイル

- `addEntry()`: アセットのエントリーポイントを指定する。たとえば、app.js ファイルがここでエントリーファイルとして指定されている。このファイルからプロジェクト内の必要なアセットが読み込まれる。

## Webpack Encore の実行

- 以下のコマンドを実行する。

```bash
yarn watch
# or
npm run watch
```

- これは `encore dev --watch` のショートカット。

- `yarn watch` は 2 つのことをする。
  - `public/build/`ディレクトリを作成する。
  - その中に `app.css` と `app.js` ファイルを作成する。
- `yarn watch` を実行すると、リアルタイムで変更が反映されるようになる。この「ライブリロード」機能によって、ブラウザをリロードするたびに手動で変更を確認する手間が省ける。
