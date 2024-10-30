# 02. Meet our Tiny App

## 学習日

- 2024/10/22

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#02 Meet our Tiny App](https://symfonycasts.com/screencast/symfony6/directories)

## ディレクトリ

### public/

- `public/` ディレクトリは、Web ルートのディレクトリで、すべての HTTP のリソースのエントリーポイントである index.php ファイルがある。

### config/ & src/

- `config/` ディレクトリは、デフォルトと注意が必要な設定の一式が入っている。各パッケージで 1 つのファイルとなる。ほとんど変更することはない。
- `src/` ディレクトリは、自分で書くコードが入る場所で、開発時のほとんどはここを使用することになる。デフォルトでは、このディレクトリに入る全てのクラスは `App` ネームスペースを使用することになる。

### composer.json & vendor/

- `vendor/` ディレクトリは Symfony 自体も含め、Composer によってインストールされたすべてのパッケージが格納される。
- このディレクトリは Composer によって管理されているので触らない。

### bin/ & var/

- `bin/` ディレクトリは、よく使う CLI コマンドの `console` が入っている。

## PhpStorm のセットアップ

- プラグイン "[Symfony Support](https://plugins.jetbrains.com/plugin/7219-symfony-support)" をインストールする。
- Setting > PHP > Symfony で以下の設定を行う。
  - `Enable Plugin for Project` をチェックする。
  - `Web Directory` に `public` を設定する。
