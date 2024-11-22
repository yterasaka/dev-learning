# 09. Querying the Database

## 学習日

- 2024/11/22

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#09 Querying the Database](https://symfonycasts.com/screencast/symfony-doctrine/queries)

## エンティティマネージャーを使用してデータベースからデータを取得する

- コントローラ内で `EntityManagerInterface` を使用してデータベースからデータを取得する。

```php
use Doctrine\ORM\EntityManagerInterface;
use App\Entity\VinylMix;

class VinylController extends AbstractController
{
    public function browse(EntityManagerInterface $entityManager, string $slug = null): Response
    {
        $genre = $slug ? u(str_replace('-', ' ', $slug))
        $mixRepository = $entityManager->getRepository(VinylMix::class);
        $mixes = $mixRepository->findAll();

        return $this->render('vinyl/browse.html.twig', [
        'genre' => $genre,
        'mixes' => $mixes,
    ]);
    }
}
```

- もしくは、リポジトリをコントローラに直接注入することもできる。

```php
use App\Repository\VinylMixRepository;

class VinylController extends AbstractController
{
    public function browse(VinylMixRepository $mixRepository, string $slug = null): Response
    {
        $genre = $slug ? u(str_replace('-', ' ', $slug))
        $mixes = $mixRepository->findAll();

        return $this->render('vinyl/browse.html.twig', [
        'genre' => $genre,
        'mixes' => $mixes,
    ]);
    }
}
```

- Twig ではエンティティのプロパティに直接アクセスするように記述できるが、内部的にはゲッターメソッド (例: getTitle()) が呼び出されている。

```twig
{% for mix in mixes %}
    <div class="mixed-vinyl-container p-3 text-center">
        <p class="mt-2"><strong>{{ mix.title }}</strong></p>
        <span>{{ mix.trackCount }} Tracks</span>
        |
        <span>{{ mix.genre }}</span>
        |
        <span>{{ mix.createdAt|ago }}</span>
    </div>
{% endfor %}
```

## リポジトリ

- リポジトリはエンティティごとに用意されるクラスで、データ取得のロジックをカプセル化する。
- リポジトリクラスは MakerBundle によって `src/Repository` ディレクトリに自動生成される。

```php
class VinylController extends AbstractController
{
    public function browse(EntityManagerInterface $entityManager, string $slug = null): Response
    {
        dd($mixRepository);
    }
}
```
