# 13. Parameters

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#13 Parameters](https://symfonycasts.com/screencast/symfony6-fundamentals/parameters)

## Parameters

- 以下のコマンドで、コンテナ内のサービスとパラメータを一覧表示することができる。

  ```bash
  php bin/console debug:container --parameters
  ```

## パラメータの使い方

### コントローラからパラメータを取得する

- コントローラからパラメータを取得するには、以下のようにして `getParameter()` メソッドを使う。

  ```php
  // ... lines 1 - 10
  class VinylController extends AbstractController
  {
  // ... lines 13 - 31
    public function browse(MixRepository $mixRepository, string $slug = null): Response
    {
        dd($this->getParameter('kernel.project_dir'));
  // ... lines 35 - 42
    }
  }
  ```

### `%parameter%` を使ってパラメータを取得する

- 以下のように、`%parameter%` を使って設定ファイルでパラメータを取得することができる。

  ```yaml
  twig:
    default_path: '%kernel.project_dir%/templates'
  // ... lines 3 - 7
  ```

## 新規パラメータの作成

- 新しいパラメータは以下のように作成する。
- yaml 設定ファイルでは、パラメータを必ずしもシングルクォートで囲む必要はないが、推奨されている。

```yaml
parameters:
    cache_adapter: 'cache.adapter.filesystem'
// ... line 3
framework:
    cache:
// ... lines 6 - 13
        app: '%cache_adapter%'
// ... lines 15 - 23
when@dev:
    parameters:
        cache_adapter: 'cache.adapter.array'
```

- 上記で新しく作成したパラメータは、以下のコマンドで確認できる。

```bash
php bin/console debug:config framework cache
```

- 本番環境の設定確認は、キャッシュクリア後、--env=prod オプションを付けて確認する。

```bash
php bin/console cache:clear --env=prod
php bin/console debug:config framework cache --env=prod
```
