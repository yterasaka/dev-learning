# 15. All About services.yaml

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#15 All About services.yaml](https://symfonycasts.com/screencast/symfony6-fundamentals/services-yaml)

## `services.yaml` の内容について

### `_defaults` セクション

- `_defaults` セクションは、サービス定義のデフォルト値を設定するためのセクション。
- この設定によって、サービス定義の繰り返しを避けることができる。
- Symfony ではデフォルトでサービスを自動ワイヤリングし、自動設定するように設定している。

```yaml
// ... lines 1 - 7
services:
    # default configuration for services in *this* file
    _defaults:
        autowire: true      # Automatically injects dependencies in your services.
        autoconfigure: true # Automatically registers your services as commands, event subscribers, etc.
// ... lines 13 - 29
```

### サービスの自動登録

- `_defaults` セクションの後に、以下のようなセクションがある。
- `exclude` は、`src` ディレクトリ内の特定のディレクトリやファイルを除外するための設定。

```yaml
// ... lines 1 - 7
services:
// ... lines 9 - 13
    # makes classes in src/ available to be used as services
    # this creates a service per class whose id is the fully-qualified class name
    App\:
        resource: '../src/'
        exclude:
            - '../src/DependencyInjection/'
            - '../src/Entity/'
            - '../src/Kernel.php'
// ... lines 22 - 29
```

## なぜ `services.yaml` を使うのか

- `services.yaml` に記述している内容は、他のファイルに記述しても問題ない。
- しかし、`services.yaml` に記述することで、サービスの設定を一元管理することができる。
- また、`services.yaml` にパラメータもまとめることで、パラメータの一元管理も可能。

```yaml
// ... lines 1 - 5
parameters:
    cache_adapter: 'cache.adapter.filesystem'
when@dev:
    parameters:
        cache_adapter: 'cache.adapter.array'
// ... lines 12 - 34
```
