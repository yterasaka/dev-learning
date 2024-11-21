# 01. Installing Doctrine

## 学習日

- 2024/11/21

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#01 Installing Doctrine](https://symfonycasts.com/screencast/symfony-doctrine/install)

## Doctrine のインストール

- Symfony ではデータベースの操作に Doctrine を使用する。
- Doctrine 以下のコマンドでインストールできる。

```bash
composer require "doctrine:^2.2" "doctrine/annotations:^1.14"
```

- インストール時に "Do you want to include Docker configuration from recipes?" と聞かれる。`p` を選択すると、永久に有効化できる。

## Doctrine Recipes

- インストール後、レシピによって以下の変更が行われる。

1. `config/bundles.php` に Doctrine 関連バンドルが追加される。

```php
return [
    Doctrine\Bundle\DoctrineBundle\DoctrineBundle::class => ['all' => true],
    Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle::class => ['all' => true],
];
```

2. `.env` ファイルにデータベース接続情報が追加される。

```.env
DATABASE_URL="postgresql://symfony:ChangeMe@127.0.0.1:5432/app?serverVersion=13&charset=utf8"
```

3. `config/packages/doctrine.yaml` ファイルが追加される。

```yaml
doctrine:
  dbal:
    url: "%env(resolve:DATABASE_URL)%"
```

4. 新しいディレクトリが追加される。

- `migration/`: マイグレーションファイルを格納する。
- `src/Entity/`: エンティティクラスを格納する。
- `src/Repository/`: リポジトリクラスを格納する。
