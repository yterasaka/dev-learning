# 07. Configuring the Cache Service

## 学習日

- 2024/11/14

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#07 Configuring the Cache Service](https://symfonycasts.com/screencast/symfony6-fundamentals/cache-config)

## Cache の設定

- Cache の設定は、`config/packages/cache.yaml` ファイルで行う。
- キャッシュサービスを提供しているのは `FrabeworkBundle`。
- 以下のコマンドで、`FrabeworkBundle` に関する情報を確認できる。

```bash
php bin/console config:dump framework
```

## Cache Config のデバッグ

- 以下のコマンドで、Cache Config をデバッグすることができる。

```bash
sole config:dump framework cache
```

## Cache Adapter の設定

- キャッシュシステムの保存先は、デフォルトでは `filesystem` になっている。

```yaml
# App related cache pools configuration
app: cache.adapter.filesystem
```

- このキャッシュアダプタを変更して、保存先を変更することができる。

- キャッシュを Redis に保存する場合は、以下のように設定する。

```yaml
framework:
  cache:
    app: cache.adapter.redis
```

- キャッシュをメモリに保存する場合は、以下のように設定する。

```yaml
framework:
  cache:
    app: cache.adapter.array
```

- 以下の表は、`cache.adapter.array` と `cache.adapter.redis` の特徴を比較したものである。

| 特徴               | `cache.adapter.array`                                        | `cache.adapter.redis`                                                  |
| ------------------ | ------------------------------------------------------------ | ---------------------------------------------------------------------- |
| **保存場所**       | メモリ内の一時的な配列に保存                                 | 外部の Redis サーバーに保存                                            |
| **永続性**         | 永続性がない（リクエスト終了後に消える）                     | 永続性がある（Redis サーバーに保存されるため、アプリの再起動後も保持） |
| **主な用途**       | 開発やテスト環境向け                                         | 本番環境向け（特に大規模なアプリケーションで推奨）                     |
| **スコープ**       | リクエストスコープ（リクエストごとに再生成される）           | アプリケーション全体で共有可能                                         |
| **パフォーマンス** | メモリ内で非常に高速（ただしスコープが限定的）               | 高速かつ永続性のあるキャッシュを提供                                   |
| **制限**           | メモリ使用量が多くなる可能性があり、データ量が多い場合に不適 | Redis サーバーの管理が必要（設定、メンテナンスなど）                   |

## 要点

- Symfony の設定ファイルはアプリ内のサービスを構成するためにあり、キーを変更することで、サービスオブジェクトのクラス名やコンストラクタ引数が変わり、インスタンス化の方法に影響を与える。
- 設定ファイル内の内容はアプリから直接読み取ることはできない。Symfony は設定を読み込んでサービスのインスタンス化を構成した後、その設定情報は破棄される。
- サービスがアプリケーションで最も重要な役割を果たす。
- 開発環境や本番環境などの異なる環境で設定が異なる場合に備えて、Symfony には「環境」（environments）というシステムが用意されている。
