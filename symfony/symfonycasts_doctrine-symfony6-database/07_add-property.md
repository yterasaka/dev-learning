# 07. Adding new Properties

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#07 Adding new Properties](https://symfonycasts.com/screencast/symfony-doctrine/add-property)

## 既存のエンティティにプロパティを追加

- 以下のコマンドで、新しいプロパティを追加する。

```bash
php bin/console make:entity
```

- エンティティ名の入力で、既存のエンティティ名を入力する。
- 新規エンティティ作成時と同じ手順で、新しいプロパティを追加する。
- 既存のエンティティファイルに新しいプロパティが追加される。

```php
class VinylMix
{
    #[ORM\Column]
    private ?int $votes = null;
    public function getVotes(): ?int
    {
        return $this->votes;
    }

    public function setVotes(int $votes): self
    {
        $this->votes = $votes;

        return $this;
    }
}
```

## 変更を加えたエンティティのマイグレーション

### マイグレーションファイルの作成

- 新規エンティティ作成時と同じ手順で、マイグレーションファイルを作成する。

```bash
symfony console make:migration
```

- 新しいマイグレーションファイルが作成され、変更内容が記述される。

```php
final class Version20220718170741 extends AbstractMigration
{
    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('ALTER TABLE vinyl_mix ADD votes INT NOT NULL');
    }
}
```

### マイグレーションの実行

- 以下のコマンドで、マイグレーションを実行する。

```bash
symfony console doctrine:migrations:migrate
```

- Doctrine は未実行のマイグレーションのみを適用する。

## プロパティのデフォルト値を設定する

- Doctrine が作成したエンティティファイルのプロパティにデフォルト値を設定する。

```php
class VinylMix
{
    // private ?int $votes = null; // Doctrine が設定したデフォルト値
    private int $votes = 0; // NULL を許容しないように '?' を削除し、デフォルト値を設定
}
```

- この変更は PHP の振る舞いのみで、データベースの構造には影響を与えない。
- 以下のコマンドで、データベースを最新の状態に保つために必要な SQL を出力して、データベースとエンティティが同期しているか確認する。

```bash
symfony console doctrine:schema:update --dump-sql
```

## createdAt の値を自動で設定する

- `__construct()` メソッドで、`createdAt` プロパティに現在日時を自動設定する。

```php
class VinylMix
{
    // ... 省略

    #[ORM\Column(nullable: true)]
    // private ?\DateTimeImmutable $createdAt = null; // Doctrine が設定したデフォルト値
    private \DateTimeImmutable $createdAt; // 自動で値がされるため NULL を許容しないように　'?' を削除し、デフォルト値を削除

    // ... 省略

    public function __construct()
    {
        $this->createdAt = new \DateTimeImmutable();
    }
}
```

-
