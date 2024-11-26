# 18. Clean URLs with Sluggable

## 学習日

- 2024/11/26

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#18 Clean URLs with Sluggable](https://symfonycasts.com/screencast/symfony-doctrine/sluggable)

## Sluggable 機能を有効にする

- `config/packages/stof_doctrine_extensions.yaml` ファイルを編集し、Sluggable 機能を有効にする。

```yaml
stof_doctrine_extensions:
  default_locale: en_US
  orm:
    default:
      timestampable: true
      sluggable: true # 追加
```

## エンティティにスラッグプロパティを追加する

- `make:entity` コマンドで新しい `slug` プロパティを追加する。

```bash
php bin/console make:entity
```

- プロパティを文字列型 (`string`) にし、最大 100 文字 (`length: 100`) に設定。
- `VinylMix` エンティティに追加された `slug` プロパティに `unique` 制約を追加する。
- これによって、同じスラッグを持つエンティティが複数存在しないようにする。

```php
#[ORM\Column(length: 100, unique: true)]
    private ?string $slug = null;
```

- マイグレーションを実行する。
- 既存データがある場合は、エラーが発生する可能性があるため、開発環境ではデータベースをリセットする。

```bash
symfony console doctrine:database:drop --force
symfony console doctrine:database:create
symfony console doctrine:migrations:migrate
```

## スラッグ生成の設定

- スラッグを自動生成するために、プロパティに `#[Slug]` 属性を追加する。
- この属性により、`title` プロパティからスラッグが自動生成される。

```php
use Gedmo\Mapping\Annotation\Slug; // 追加

// ...

#[ORM\Column(length: 100, unique: true)]
#[Slug(fields: ['title'])] // 追加
private ?string $slug = null;
```

- データベースを確認して、スラッグが適切に生成されていることを確認する。
- `-1`, `-2` のようにユニーク性を保つためのサフィックスが付加される。

```bash
symfony console doctrine:query:sql 'SELECT * FROM vinyl_mix
```

## ルートのスラッグ対応

- `MixController` の `show` アクションのルートを更新する。

```php
#[Route('/mix/{slug}', name: 'app_mix_show')] // 変更
    public function show(VinylMix $mix): Response
    {
        return $this->render('mix/show.html.twig', [
            'mix' => $mix,
        ]);
    }
```

- スラッグを使用するようコントローラーでのリダイレクトを更新する。

```php
#[Route('/mix/{id}/vote', name: 'app_mix_vote', methods: ['POST'])]
    public function vote(VinylMix $mix, Request $request, EntityManagerInterface $entityManager): Response
    {
        $direction = $request->request->get('direction', 'up');

        if ($direction === 'up') {
            $mix->upVote();
        } else {
            $mix->downVote();
        }

        $entityManager->flush();
        $this->addFlash('success', 'Vote counted!');

        return $this->redirectToRoute('app_mix_show', ['slug' => $mix->getSlug()]); // 変更
    }
```

- スラッグを使用するようテンプレートでのリンク生成を更新する。

```twig
<!-- ... -->
{% for mix in mixes %}
  <div class="col col-md-4">
      <a href="{{ path('app_mix_show', { slug: mix.slug }) }}" class="mixed-vinyl-container p-3 text-center"> <!-- 変更 -->
          <img src="{{ mix.getImageUrl(300) }}" alt="Mix album cover">
          <p class="mt-2"><strong>{{ mix.title }}</strong></p>
          <span>{{ mix.trackCount }} Tracks</span>
          |
          <span>{{ mix.genre }}</span>
          |
          <span>{{ mix.createdAt|ago }}</span>
          <br>
          {{ mix.getVotesString() }} votes
      </a>
  </div>
{% endfor %}
<!-- ... -->
```

- ブラウザでサイトをリロードして確認する。
