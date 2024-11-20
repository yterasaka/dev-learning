# 22. Reading Secrets vs Env Vars

## 学習日

- 2024/11/20

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#22 Reading Secrets vs Env Vars](https://symfonycasts.com/screencast/symfony6-fundamentals/secrets-usage)

## 本番用の Secrets Vault の設定

- 本番環境用の Secrets Vault は、以下のコマンドで設定する。

```bash
php bin/console secrets:set GITHUB_TOKEN --env=prod

# シークレット値を入力する
```

- 上記コマンドを実行すると、`config/secrets/prod/` ディレクトリに暗号化されたファイルが生成される。
- 開発用の Vault とは異なり、本番環境用の Vault には復号鍵 (`prod.decrypt.private.php`) が `.gitignore` に含まれているため、コミットされない。

- 以下のコマンドで作成されたファイルをコミットすることができる。

```bash
git add config/secrets/prod
git commit -m "Add prod secrets vault"
```

## Secrets の優先順位

- Secrets Vault の値は、環境変数として読み取られる。ただし、環境変数 (`.env`, `.env.local`) は Vault の値より優先される。
- そのため、Secrets Vault を使用する場合は、`.env` や `.env.local` に同じキーが存在しないようにする。

## Secrets Vault の確認とデバッグ

- Secrets Vault に保存された値を確認するには、以下のコマンドを実行する。

```bash
php bin/console secrets:list
```

- 上記のコマンドでは値が表示されないため、値を確認するには以下のコマンドを実行する。

```bash
php bin/console secrets:list --reveal
```

- 本番環境の Secrets Vault にアクセスするには、以下のコマンドを実行する。本番環境の Secrets Vault には復号鍵が含まれていないため、値を表示することはできない。

```bash
php bin/console secrets:list --reveal --env=prod
```

## 開発時にローカルで本番用のトークンを使用する

- 開発環境で "ダミーの値" を Vault に設定している場合、`.env.local` で一時的に本番用のトークンを設定して作業を続ける。
- 作業終了後、`.env.local` の内容を戻すことを忘れない。

## トークンの値をブラウザで確認する

- 以下のようにして、トークンの値をブラウザで確認できる。

```php
class MixRepository
{
    public function findAll(): array
    {
        dd($this->githubContentClient);
    }
}
```

- ブラウザで dump された領域をクリックして、`cmd または ctrl + F` を押して検索することで、トークンの値を確認できる。
