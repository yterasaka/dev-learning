# 21. The Secrets Vault

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#21 The Secrets Vault](https://symfonycasts.com/screencast/symfony6-fundamentals/secrets-vault)

## Symfony アプリケーションのデプロイ手順

1. コードを本番サーバに転送
   - コミット済みのコードを本番サーバーに転送し、サーバー上で `composer install` を実行する。

```bash
composer install
```

2. 環境変数を設定
   - `.env.local` ファイルを本番環境で作成し、環境変数を設定する。
   - その際に、`APP_ENV=prod` を設定し、本番環境であることを明示する。

```.env
APP_ENV=prod
APP_SECRET=<アプリのシークレットキー>
DATABASE_URL=<データベース接続URL>
```

- サーバー側で環境変数を直接設定できる場合、`.env.local` を作成する必要はない。Symfony は自動的に環境変数を読み込む。

3. キャッシュクリア&ウォームアップ
   - 本番環境でキャッシュをクリアし、ウォームアップする。

```bash
php bin/console cache:clear
php bin/console cache:warmup
```

## Secrets Vault

- 機密情報（API キーやデータベース情報など）は、`.env.local` に直接記載する代わりに Symfony の Secrets Vault を利用することもできる。
- Secrets Vault は機密情報を暗号化して保存するため、Git リポジトリに安全にコミット可能。

### 開発用 Secrets Vault の設定

- Secrets Vault は開発用と本番用の 2 つの設定が必要。
- `dev` 環境用は、以下の手順で設定する。

```bash
php bin/console secrets:set GITHUB_TOKEN

# シークレット値を入力する
```

- 上記コマンドを実行すると、`config/secrets/dev/` ディレクトリに暗号化されたファイルが生成される。
- この Vault は開発用であるため、復号鍵を含めても問題ない。
- 本番環境用の Vault では、復号鍵を含めてはいけない。
