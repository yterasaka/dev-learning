# 08. Persisting to the Database

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#08 Persisting to the Database](https://symfonycasts.com/screencast/symfony-doctrine/persist)

## 新しいコントローラークラスを作成

- `src/Controller/MixController.php` を作成し、`AbstractController` を継承する。
- 新しいミックスを作成する `new()` メソッドを追加する。

```php
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MixController extends AbstractController
{
    #[Route('/mix/new')]
    public function new(): Response
    {

    }
}
```

## オブジェクトをインスタンス化する

- `new()` メソッド内で、`VinylMix` エンティティのオブジェクトをインスタンス化する。

```php
    public function new(): Response
    {
        $mix = new VinylMix();
        $mix->setTitle('Do you Remember... Phil Collins?!');
        $mix->setDescription('A pure mix of drummers turned singers!');
        $mix->setGenre('pop');
        $mix->setTrackCount(rand(5, 20));
        $mix->setVotes(rand(-50, 50));

        dd($mix);
    }
```

## オブジェクトをデータベースに保存

- データベースにオブジェクトを保存するのは `EntityManagerInterface` が担当する。

```php
public function new(EntityManagerInterface $entityManager): Response
{
    $mix = new VinylMix();
    $mix->setTitle('Do you Remember... Phil Collins?!');
    $mix->setDescription('A pure mix of drummers turned singers!');
    $mix->setGenre('pop');
    $mix->setTrackCount(rand(5, 20));
    $mix->setVotes(rand(-50, 50));

    $entityManager->persist($mix);
    $entityManager->flush();

    return new Response(sprintf(
        'Mix %d is %d tracks of pure 80\'s heaven',
        $mix->getId(),
        $mix->getTrackCount()
    ));
}
```

### `persist()` と `flush()` の役割

- `persist($mix)` は、Doctrine に「このオブジェクトを追跡してほしい」と伝えるが、データベースには即座に保存されない。
- `flush()` を呼び出すと、追跡中のオブジェクトをすべてデータベースに保存する。

## データベースに保存されたオブジェクトを確認

- 以下のコマンドで、データベースに保存されたオブジェクトを確認できる。

```bash
symfony console doctrine:query:sql 'SELECT * FROM vinyl_mix'
```

```bash
# Output
---- ----------------------------------- ---------------------------------------- ------------- ------- --------------------- -------
  id   title                               description                              track_count   genre   created_at            votes
 ---- ----------------------------------- ---------------------------------------- ------------- ------- --------------------- -------
  1    Do you Remember... Phil Collins?!   A pure mix of drummers turned singers!   17            pop     2024-11-22 10:09:45   -24
  2    Do you Remember... Phil Collins?!   A pure mix of drummers turned singers!   16            pop     2024-11-22 10:09:49   26
 ---- ----------------------------------- ---------------------------------------- ------------- ------- --------------------- -------
```

- Doctrine は保存時に新しい ID を生成し、エンティティの id プロパティに自動的に設定する。
