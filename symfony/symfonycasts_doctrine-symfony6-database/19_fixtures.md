# 19. Simple Doctrine Data Fixtures

## 学習日

- 2024/11/27

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#19 Simple Doctrine Data Fixtures](https://symfonycasts.com/screencast/symfony-doctrine/fixtures)

## DoctrineFixturesBundle

- `DoctrineFixturesBundle` は、Doctrine でデータフィクスチャを管理するための Symfony バンドル。
- データフィクスチャとは、アプリケーション開発やテストの際に、データベースに投入するためのダミーデータや初期データを指す。
- 以下のコマンドで `DoctrineFixturesBundle` をインストールする。

```bash
composer require --dev orm-fixtures
```

- コマンドの実行後、バンドルと `src/DataFixtures` ディレクトリが追加される。このディレクトリの中には `AppFixtures.php` というファイルが含まれる。

## `load()` メソッド

- `load()` メソッドは、データフィクスチャをロードするためのメソッド。
- このメソッド内で、データベースにデータを投入する処理を記述する。

```php
use App\Entity\VinylMix;
// ...
class AppFixtures extends Fixture
{
    public function load(ObjectManager $manager): void
    {
        $mix = new VinylMix();
        $mix->setTitle('Do you Remember... Phil Collins?!');
        $mix->setDescription('A pure mix of drummers turned singers!');
        $genres = ['pop', 'rock'];
        $mix->setGenre($genres[array_rand($genres)]);
        $mix->setTrackCount(rand(5, 20));
        $mix->setVotes(rand(-50, 50));

        $manager->persist($mix);
        $manager->flush();
    }
}
```

- 次に、以下のコマンドを実行してデータフィクスチャをロードする。

```bash
symfony console doctrine:fixtures:load
```

---

参照

- [DoctrineFixturesBundle](https://symfony.com/bundles/DoctrineFixturesBundle/current/index.html)
