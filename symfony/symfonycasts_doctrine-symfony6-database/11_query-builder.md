# 11. The Query Builder

## 学習日

- 2024/11/25

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#11 The Query Builder](https://symfonycasts.com/screencast/symfony-doctrine/query-builder)

## ジャンルでデータをフィルタリングする

- findBy() メソッドの第一引数に検索条件を指定することで、データをフィルタリングできる。

```php
$mixes = $mixRepository->findBy(['genre' => 'pop'], ['votes' => 'DESC']);
```

## QueryBuilder

- もしくは、QueryBuilder を使うことで、より複雑なクエリを実行できる。
- Doctrine は独自のクエリ言語（DQL）を持っている。DQL を直接書くこともできるが、QueryBuilder を使うことで、より安全で読みやすいクエリを書くことができる。
- 'VinylMixRepository' クラスに、以下のようなメソッドを追加する。

```php
/**
    * @return VinylMix[] Returns an array of VinylMix objects
    */
   public function findAllOrderedByVotes(string $genre = null): array
   {
        $queryBuilder = $this->createQueryBuilder('mix')
        ->orderBy('mix.votes', 'DESC');

        if ($genre) {
            $queryBuilder->andWhere('mix.genre = :genre')
            ->setParameter('genre', $genre);
        }

        return $queryBuilder
           ->getQuery()
           ->getResult()
        ;
   }
```

- `Where` ステートメントを追加する場合は、常に `andWhere` メソッドを使う。それによって、クエリの構造が破壊されるのを防ぐことができる。
- 'andWhere' には動的な文字列を直接入れない。代わりに、プレースホルダ :genre に値をバインドすることで、SQL インジェクションを防ぐことができる。

- 'VinylController' クラスでメソッドに引数を渡す。

```php
#[Route('/browse/{slug}', name: 'app_browse')]
    public function browse(VinylMixRepository $mixRepository, string $slug = null): Response
    {
        $genre = $slug ? u(str_replace('-', ' ', $slug))->title(true) : null;
        $mixes = $mixRepository->findAllOrderedByVotes($slug); // 引数を渡す

        return $this->render('vinyl/browse.html.twig', [
            'genre' => $genre,
            'mixes' => $mixes,
        ]);
    }
```

- クエリの内容は Symfony Profiler の 'Doctrine' タブで確認できる。

## QueryBuilder ロジックの再利用

- 複数のクエリで同じロジックを使いたい場合は、QueryBuilder を再利用できるようにメソッド化することができる。

```php
// 再利用できるようにメソッド化する
private function addOrderByVotesQueryBuilder(QueryBuilder $queryBuilder = null) {
    $queryBuilder = $queryBuilder ?? $this->createQueryBuilder('mix');

    return $queryBuilder->orderBy('mix.votes', 'DESC');
}

// メソッド化した queryBuilder を使ってクエリを実行する。
/**
* @return VinylMix[] Returns an array of VinylMix objects
*/
public function findAllOrderedByVotes(string $genre = null): array
{
    $queryBuilder = $this->addOrderByVotesQueryBuilder();

    if ($genre) {
        $queryBuilder->andWhere('mix.genre = :genre')
        ->setParameter('genre', $genre);
    }

    return $queryBuilder
        ->getQuery()
        ->getResult()
    ;
}
```

---

参考

- [Databases and the Doctrine ORM](https://symfony.com/doc/current/doctrine.html)
