# 01. Hello Symfony

## 学習日

- 2024/10/21

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#01 Hello Symfony](https://symfonycasts.com/screencast/symfony6/setup)

## Symfony バイナリのインストール

- コマンドラインツール
- [Download Symfony](https://symfony.com/download) からインストール方法を確認する。

```bash
# mac
brew install symfony-cli/tap/symfony-cli

# ubuntu
curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
sudo apt install symfony-cli
```

正しくインストールされていれば、以下のコマンドでバージョンが実行できる。

```bash
symfony
```

以下のコマンドで、symfony で実行できるすべての機能を確認できる。

```bash
symfony list
```

## 新規プロジェクトの作成

```bash
symfony new my_project
```

- プロジェクトが作成されると、git リポジトリも初期化される。
- より機能が充実したプロジェクトから介したい場合は、以下のコマンドを実行する。

```bash
symfony new　my_project　--webapp
```

- Docker 化された新しい Symfony アプリが必要な場合は、以下を確認する
  [Symfony Docker](https://github.com/dunglas/symfony-docker)

## システム要件の確認

```bash
symfony check:req
```

## 開発用 Web サーバーの起動・停止・ステータス確認

```bash
# 起動
symfony serve -d

# 停止
symfony server:stop

# ステータス確認
symfony server:status
```
