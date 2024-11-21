# 03. docker-env-vars

## 学習日

- 2024/11/21

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#03 Docker & Environment Variables](https://symfonycasts.com/screencast/symfony-doctrine/docker-env-vars)

## Symfony Binary & Docker Env Vars

- Symfony バイナリは、Docker コンテナを検出し、自動的に適切な環境変数を設定する。この機能によって、ポート番号やデータベースの接続情報などを設定する必要がなくなる。
- 以下のコマンドで、Symfony バイナリが設定したすべての環境変数を確認できる。

```bash
symfony var:export --multiline
```
