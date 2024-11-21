# 02. docker-compose & Exposed Ports

## 学習日

- 2024/11/21

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#02 docker-compose & Exposed Ports](https://symfonycasts.com/screencast/symfony-doctrine/docker-compose)

## データベース用の Docker コンテナ

- Doctrine のインストール時に Docker の設定を有効化すると、以下の設定で `compose.yaml` ファイルが生成される。

```yaml
# compose.yaml

version: "3"

services:
  ###> doctrine/doctrine-bundle ###
  database:
    image: postgres:${POSTGRES_VERSION:-13}-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-app}
      # You should definitely change the password in production
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-ChangeMe}
      POSTGRES_USER: ${POSTGRES_USER:-symfony}
    volumes:
      - db-data:/var/lib/postgresql/data:rw
```

```yaml
# compose.override.yaml

version: "3"

services:
  ###> doctrine/doctrine-bundle ###
  database:
    ports:
      - "5432"
```

### コンテナの起動

- 以下のコマンドで Docker コンテナを起動する。

```bash
docker compose up -d
```

- `-d` オプションを付けると、バックグラウンドでコンテナが起動する。
- 初回実行時は PostgreSQL のイメージがダウンロードされる。

### コンテナの確認

```bash
docker compose ps
```

- 出力例: ホストマシンのポート（例: 50700）が Postgres の標準ポート（5432）にマッピングされている。

```bash
Name                Command               State             Ports
----------------------------------------------------------------------------------
database   docker-entrypoint.sh postgres   Up      0.0.0.0:50700->5432/tcp
```

### データベースへの接続

```bash
docker compose exec database psql --user app --password app
```

### コンテナの停止

- 一時停止

```bash
docker compose stop
```

- 停止 & コンテナ削除

```bash
docker compose down
```
