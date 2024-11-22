# 05. Entity Class

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#05 Entity Class](https://symfonycasts.com/screencast/symfony-doctrine/entity)

## Entity とは

- Entity は、データベースのテーブルに対応する PHP クラス。
- プロパティはデータベースのカラムに対応し、Doctrine が自動的にそれらを管理する。
- Entity を使うことで、データベースを「オブジェクト」として扱うことができる。

## Entity Class の作成

### コマンドの実行

- 以下のコマンドで、Entity Class を作成する。

```bash
php bin/console make:entity
```

- `symfony console` は不要。（このコマンドはデータベースに接続しないため）

### プロパティの定義

- プロパティ名、型、長さ、NULL 許可を入力すると、Entity Class が作成される。
- 例:

  - title (string, 必須)
  - description (text, 任意)
  - trackCount (integer, 必須)
  - genre (string, 必須)
  - createdAt (datetime_immutable, 必須)

### Entity Class の内容

- Entity Class が作成されると、`src/Entity` ディレクトリにファイルが保存される。

```php
#[ORM\Entity(repositoryClass: VinylMixRepository::class)]
class VinylMix
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column()]
    private ?int $id = null;

    #[ORM\Column(length: 255)]
    private ?string $title = null;

    #[ORM\Column(type: Types::TEXT, nullable: true)]
    private ?string $description = null;
    public function getId(): ?int
    {
        return $this->id;
    }

    public function getTitle(): ?string
    {
        return $this->title;
    }

    public function setTitle(string $title): self
    {
        $this->title = $title;

        return $this;
    }
}
```

- `#[ORM\Entity]`: このクラスが Entity であることを Doctrine に伝える。
- `#[ORM\Column]`: 各プロパティがデータベースのカラムに対応することを Doctrine に伝える。
- 型推論: プロパティの型からカラムの型を自動的に推測します（例：`?string` → `string`）。

### 命名規則

- テーブル名はクラス名をスネークケースに変換したものになる。（例: `VinylMIx` > `vinyl_mix`）
- プロパティ名も同様にスネークケースに変換される。（例: `$trackCount` > `track_count`）
