# 06. Bundle Config

## 学習日

- 2024/11/14

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#06 Bundle Config](https://symfonycasts.com/screencast/symfony6-fundamentals/bundle-config)

## Bundle Configuration

- `config/packages` ディレクトリにあるファイルは、Symfony が最初に読み込む設定ファイルである。
- これらのファイルは、バンドルが提供するサービスを設定するために使用される。
- `yaml` ファイルの名前は基本的に設定上の動作には影響を与えない。
- 設定の適用は root key に基づいて行われる。

- 設定の例:

```yaml
#  %kernel.project_dir% は、プロジェクトのルートディレクトリを表す。
twig:
  default_path: "%kernel.project_dir%/templates"

when@test:
  twig:
    strict_variables: true
```

## 設定可能な Bundle Config のデバッグ

- 以下のコマンドで、設定可能な Bundle Config をデバッグすることができる。

```bash
# 登録されているバンドルのリストを表示する。
php bin/console debug:config

# 特定のバンドルの設定を表示する。
php bin/console debug:config twig
```

- 以下のコマンドで、設定可能な Bundle Config のデフォルト値を表示することができる。

```bash
# 登録されているバンドルのリストを表示する。
php bin/console config:dump

# 特定のバンドルの設定を表示する。
php bin/console config:dump twig
```
