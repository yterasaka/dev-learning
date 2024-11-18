# 10. The "prod" Environments

## 学習日

- 2024/11/18

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#19 The "prod" Environments](https://symfonycasts.com/screencast/symfony6-fundamentals/prod-environment)

## `prod` 環境

- `prod` 環境では、特定のファイルを変更したときに、自動的にキャッシュが再構築されない。
- そのため、以下のコマンドでキャッシュを再構築する必要がある。

```bash
php bin/console cache:clear
```

## キャッシュアダプターの変更

- キャッシュアダプターの変更は、`config/packages/cache.yaml` で行う。
- デフォルトのキャッシュアダプターは、`cache.adapter.filesystem` である。

```yaml
# 設定例
framework:
    cache:
// ... lines 3 - 10
        app: cache.adapter.filesystem
// ... lines 12 - 20
when@dev:
    framework:
        cache:
            app: cache.adapter.array
```

- 設定を変更した後は、キャッシュを再構築する。
