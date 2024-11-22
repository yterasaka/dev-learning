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

## 対話の内容

1. エンティティのクラス名を入力する。
2. Symfony UX Turbo を使用するかどうかを選択する。
   1. デフォルトは「いいえ」
   2. エンターキーを押してスキップする。
3. プロパティを設定する。

### プロパティの定義

- 以下の順でプロパティを設定する。
  1. プロパティ名
  2. プロパティの型
     1. ここで `?` を付けると、設定可能なプロパティの一覧が表示される。
  3. 長さが設定できるプロパティの場合、長さを入力する。
  4. プロパティが必須かどうか
  5. プロパティの長さ（文字列の場合）
  6. プロパティが null かどうか
  7. 次のプロパティを追加
- Entity Class のプロパティ例:
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
