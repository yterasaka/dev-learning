# 06. Migrations

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#06 Migrations](https://symfonycasts.com/screencast/symfony-doctrine/migrations)

## マイグレーションとは

- エンティティを作成した後、対応するテーブルをデータベースに反映するためにマイグレーションを使用する。

## マイグレーションファイルの作成

- 以下のコマンドで、マイグレーションファイルを作成する。

```bash
symfony console make:migration
```

- このコマンドを実行すると、`src/migrations` ディレクトリにマイグレーションファイルが作成される。
- このコマンドは現在のエンティティの状態とデータベースの構造を比較し、必要な変更を記述したマイグレーションファイルを生成する。

## マイグレーションファイルの内容

```php
final class Version20220718170654 extends AbstractMigration
{
    public function getDescription(): string
    {
        return '';
    }

    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE SEQUENCE vinyl_mix_id_seq INCREMENT BY 1 MINVALUE 1 START 1');
        $this->addSql('CREATE TABLE vinyl_mix (id INT NOT NULL, title VARCHAR(255) NOT NULL, description TEXT DEFAULT NULL, track_count INT NOT NULL, genre VARCHAR(255) NOT NULL, created_at TIMESTAMP(0) WITHOUT TIME ZONE NOT NULL, PRIMARY KEY(id))');
        $this->addSql('COMMENT ON COLUMN vinyl_mix.created_at IS \'(DC2Type:datetime_immutable)\'');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE SCHEMA public');
        $this->addSql('DROP SEQUENCE vinyl_mix_id_seq CASCADE');
        $this->addSql('DROP TABLE vinyl_mix');
    }
}
```

- `up()`: テーブル作成の SQL。
- `down()`: ロールバック処理用。

## テーブルの確認

- マイグレーションファイルを作成した後、データベースにテーブルが作成されていることを確認する。

```bash
symfony console doctrine:query:sql 'SELECT * FROM vinyl_mix'
```

- 現在は、マイグレーションファイルがまだ適用されていないため、エラーが発生する。

## マイグレーションの実行

- 以下のコマンドで、マイグレーションを実行する。

```bash
symfony console doctrine:migrations:migrate
```

- 確認を求められるので、`yes` と入力してマイグレーションを実行する。

- もう一度テーブルを確認すると、エラーが発生せず、テーブルが作成されていることが確認できる。

```bash
symfony console doctrine:query:sql 'SELECT * FROM vinyl_mix'
```

## マイグレーションの仕組み

- 実行済みのマイグレーションの管理
  - Doctrine はデータベースに `doctrine_migration_versions` という特別なテーブルを作成し、実行済みのマイグレーションを記録する。
  - 再度 `doctrine:migrations:migrate` を実行しても、未実行のマイグレーションのみ実行される。

### マイグレーションの状態管理

- 以下のコマンドで、マイグレーションの状態を確認できる。

```bash
symfony console doctrine:migrations:status
```

- 以下のコマンドで、実行済みのマイグレーションと実行日を確認できる。

```bash
symfony console doctrine:migrations:list
```
