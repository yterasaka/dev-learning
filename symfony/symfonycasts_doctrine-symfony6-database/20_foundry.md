# 20. Foundry: Fixtures You'll Love

## 学習日

- 2024/11/27

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#20 Foundry: Fixtures You'll Love](https://symfonycasts.com/screencast/symfony-doctrine/foundry)

## Foundry

- データフィクスチャを効率的に作成するために、Foundry というライブラリを使用することができる。

## Foundry のインストール

- 以下のコマンドで Foundry をインストールする。

```bash
composer require zenstruck/foundry --dev
```

## Factories: make:factory

- Foundry を使うには、各エンティティに対応する「ファクトリークラス」を作成する必要がある。
- 以下のコマンドでファクトリークラスを作成する。

```bash
php bin/console make:factory

# ファクトリーを生成するエンティティを選択
```

- 生成されたファクトリークラスは、`src/Factory` ディレクトリに保存される。

## ファクトリークラスの内容

- 生成された `VinylMixFactory` クラスには、エンティティを生成するためのテンプレート（`getDefaults()`）が記述されている。
- 以下の例のようにテンプレートを編集して、データフィクスチャを作成する。

```php
final class VinylMixFactory extends ModelFactory
{
    protected function getDefaults(): array
    {
        return [
            'title' => self::faker()->words(5, true),
            'trackCount' => self::faker()->numberBetween(5, 20),
            'genre' => self::faker()->randomElement(['pop', 'rock']),
            'votes' => self::faker()->numberBetween(-50, 50),
            'description' => self::faker()->paragraph(),
        ];
    }
}
```

- `self::faker()`: Faker ライブラリを利用して、ランダムなデータを生成する。
  - `words(5, true)` → 5 つの単語を含むタイトルを生成。
  - `numberBetween(5, 20)` → 5〜20 のランダムな数値を生成。
  - `randomElement(['pop', 'rock'])` → "pop"か"rock"をランダムに選択。

## データフィクスチャでファクトリーを使用する

- `AppFixtures.php` を編集して、ファクトリーを使用する。

```php
namespace App\DataFixtures;

use App\Factory\VinylMixFactory;
use Doctrine\Bundle\FixturesBundle\Fixture;

class AppFixtures extends Fixture
{
    public function load(): void
    {
        // 25件のVinylMixを作成
        VinylMixFactory::createMany(25);
    }
}
```

- これで、`doctrine:fixtures:load` コマンドを実行すると、25 件のダミーデータが生成される。

```bash
symfony console doctrine:fixtures:load
```

---

参照

- [Foundry](https://symfony.com/bundles/ZenstruckFoundryBundle/current/index.html)
