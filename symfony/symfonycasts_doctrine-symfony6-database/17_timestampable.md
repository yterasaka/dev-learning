# 17. Doctrine Extensions: Timestampable

## 学習日

- 2024/11/26

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#17 Doctrine Extensions: Timestampable](https://symfonycasts.com/screencast/symfony-doctrine/timestampable)

## `stof/doctrine-extensions-bundle` のインストール

- `stof/doctrine-extensions-bundle` を使うことで、Doctrine に拡張機能を追加できる。
- このバンドルに含まれているレシピは 'conttrib' リポジトリから提供されている。これは、コミュニティによってメンテナンスされているパッケージのこと。

```bash
composer require stof/doctrine-extensions-bundle
```

## タイムスタンプを有効にする

- `stof/doctrine-extensions-bundle` によって作成された `stof_doctrine_extensions.yaml` ファイルを編集し、タイムスタンプを有効にする。

```yaml
stof_doctrine_extensions:
  default_locale: en_US
  orm:
    default:
      timestampable: true
```

- 対象のエンティティに `TimestampableEntity` トレイトを追加する。
- トレイトは `createdAt` と `updatedAt` プロパティを追加し、それぞれ挿入時・更新時に自動設定される。
- これにより、手動で設定していた `createdAt` プロパティやメソッドは不要になる。

```php
use Gedmo\Timestampable\Traits\TimestampableEntity;
// ... lines 9 - 10
class VinylMix
{
    use TimestampableEntity;
// ... lines 14 - 124
}
```

- これで、エンティティが作成された日時と更新された日時が自動的に設定される。

## データベースのマイグレーション

- `stof/doctrine-extensions-bundle` によって追加されたカラムをデータベースに反映するために、マイグレーションを実行する。

```bash
symfony console make:migration
```

- しかし、そのままマイグレーションを実行すると、エラーが発生する。
- これは、既存のデータベースに多くのレコードが存在するため、NULL を許可しないカラムを追加しようとすると、エラーが発生するため。
- マイグレーションが失敗した場合の処理について、詳しくは [Timestampable & Failed Migrations](https://symfonycasts.com/screencast/symfony5-doctrine/bad-migrations) を参照する。
- ローカル環境では、既存のデータベースを削除してからマイグレーションを実行する。

```bash
# データベースを削除
symfony console doctrine:database:drop --force

# データベースを作成
symfony console doctrine:database:create

# マイグレーションを実行
symfony console doctrine:migrations:migrate
```

- 以下のコマンドで、マイグレーションが成功したことを確認する。

```bash
symfony console doctrine:query:sql 'SELECT * FROM vinyl_mix WHERE id = 7' # id の値は適宜変更する
```
