# 04. The "symfony console" Command & server_version

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#04 The "symfony console" Command & server_version](https://symfonycasts.com/screencast/symfony-doctrine/console)

## bin/console doctrine:database:create

- `php bin/console` でコマンド一覧を確認すると、`doctrine:` で始まるコマンドが追加されている。
- `doctrine:database:create` コマンドを実行すると、データベースが作成される。

```bash
php bin/console doctrine:database:create
```

- しかし、このコマンドを実行すると、エラーが発生する。
- その理由は、`bin/console` 実行時に `.env` ファイルの `DTABASE_URL` 環境変数が使用され、Docker の環境変数が無視されるため。

- この問題を解決するために、`symfony console` コマンドを使用する。
- `symfony console` を使うことで、Symfony バイナリが Docker の環境変数を適用し、正しいポートで接続する。

```bash
symfony console doctrine:database:create
```

## `server_version` の設定

- `config/packages/doctrine.yaml` ファイルに、`server_version` を設定することで、MySQL のバージョンを指定できる。

```yaml
doctrine:
  dbal:
    server_version: "13"
```

- `server_version` は、使用しているデータベースエンジンのバージョンを Doctrine に伝えるために使用される。
- データベースのバージョンごとの機能差異に対応するため、正しいバージョンを指定する。
- 例: PostgreSQL なら `13`、MySQL なら `8` や `5.7` など。
