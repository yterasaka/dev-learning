# 21. Pagination

## 学習日

- 2024/11/27

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#21 Pagination](https://symfonycasts.com/screencast/symfony-doctrine/pagination)

## Pagerfanta

- `Pagerfanta` ライブラリを使用することで、ページネーションを実装できる。
- 以下のコマンドで `Pagerfanta` をインストールする。

```bash
composer require babdev/pagerfanta-bundle pagerfanta/doctrine-orm-adapter
```

## QueryBuilder の返却

- ページネーションの準備として、`QueryBuilder` を返すリポジトリメソッドを作成する。
- 既存の `src/Repository/VinylMixRepository.php` にある `findAllOrderedByVotes()` メソッドを以下のように変更する

```php
use Doctrine\ORM\QueryBuilder;
// ...
public function createOrderedByVotesQueryBuilder(string $genre = null): QueryBuilder
{
    $queryBuilder = $this->createQueryBuilder('mix')
        ->orderBy('mix.votes', 'DESC');

    if ($genre) {
        $queryBuilder->andWhere('mix.genre = :genre')
            ->setParameter('genre', $genre);
    }

    return $queryBuilder;
}
```

## コントローラーでのページネーションの初期化

- 次に、コントローラーで `QueryBuilder` を使用してページネーションを実装する。
- `browse()` メソッドでクエリビルダーを取得し、Pagerfanta を使ってページネーションを初期化する。

```php
use Pagerfanta\Doctrine\ORM\QueryAdapter;
use Pagerfanta\Pagerfanta;
// ...
#[Route('/browse/{slug}', name: 'app_browse')]
    public function browse(VinylMixRepository $mixRepository, string $slug = null): Response
    {
        $genre = $slug ? u(str_replace('-', ' ', $slug))->title(true) : null;

        // クエリビルダーの取得
        $queryBuilder = $mixRepository->createOrderedByVotesQueryBuilder($slug);
        // ページネーションのセットアップ
        $adapter = new QueryAdapter($queryBuilder);
        $pagerfanta = Pagerfanta::createForCurrentPageWithMaxPerPage(
            $adapter,
            $request->query->get('page', 1),
            9
        );

        return $this->render('vinyl/browse.html.twig', [
            'genre' => $genre,
            'pager' => $pagerfanta, // テンプレートに渡す
        ]);
    }
```

## テンプレートでのページネーションの表示

- 最後に、Twig テンプレートでページネーションを表示する。

```twig
<div class="row">
    {% for mix in pager %} <!-- pager は Pagerfanta オブジェクト -->
        <div class="col col-md-4">
            <!-- 各アイテムの表示 -->
            <h3>{{ mix.title }}</h3>
        </div>
    {% endfor %}
</div>

<!-- ページネーションリンク -->
{{ pagerfanta(pager) }} <!-- pagerfanta 関数を使用 -->
```

## Pagerfanta の Twig テンプレートサポート

- 以下のコマンドを実行することで、Pagerfanta の Twig テンプレートサポートをインストールできる。

```bash
composer require pagerfanta/twig
```

## Bootstrap スタイルの適用

- スタイルを設定するために新しい設定ファイルを作成する。
- `config/packages/babdev_pagerfanta.yaml` を作成し、以下の内容を追加します：

```bash
cd config/packages/babdev_pagerfanta.yaml
```

```yaml
babdev_pagerfanta:
  default_view: twig
  default_twig_template: "@BabDevPagerfanta/twitter_bootstrap5.html.twig"
```

- 設定ファイルを作成したら、キャッシュをクリアする。

```bash
php bin/console cache:clear
```
