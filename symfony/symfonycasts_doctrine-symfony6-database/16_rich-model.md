# 16. Flash Message & Rich vs Anemic Models

## 学習日

- 2024/11/26

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#16 Flash Message & Rich vs Anemic Models](https://symfonycasts.com/screencast/symfony-doctrine/rich-model)

## Flash Message

- ユーザーにメッセージを表示するために、`addFlash()` メソッドを使う。
- コントローラー内で `addFlash()` を使用すると、メッセージがセッションに保存される。
- セッションに保存されたメッセージは、一度表示されると自動的に削除される。

```php
class MixController extends AbstractController
{
   // ...

    #[Route('/mix/{id}/vote', name: 'app_mix_vote', methods: ['POST'])]
    public function vote(VinylMix $mix, Request $request, EntityManagerInterface $entityManager): Response
    {
        $direction = $request->request->get('direction', 'up');

        if ($direction === 'up') {
            $mix->setVotes($mix->getVotes() + 1);
        } else {
            $mix->setVotes($mix->getVotes() - 1);
        }

        $entityManager->flush();
        // 以下を追加
        $this->addFlash('success', 'Vote counted');

        return $this->redirectToRoute('app_mix_show', ['id' => $mix->getId()]);
    }
}
```

- `templates/base.html.twig` に以下のコードを追加することで、メッセージを表示できる。
- ベーステンプレートに配置することで、全ページでフラッシュメッセージを表示できる。

```twig
    <body>
        <div class="mb-5">
            {% for message in app.flashes('success') %}
                <div class="alert alert-success">
                    {{ message }}
                </div>
            {% endfor %}
        </div>
    </body>
```

### `upVote()` と `downVote()` メソッドの追加

- 'up' または 'down' の方向に投票するためのメソッドをエンティティ (src/Entity/VinylMix.php) に追加する。

```php
public function upVote(): void
{
    $this->votes++;
}

public function downVote(): void
{
    $this->votes--;
}
```

- `MixController` クラスの `vote()` メソッドを以下のように修正する。

```php
#[Route('/mix/{id}/vote', name: 'app_mix_vote', methods: ['POST'])]
    public function vote(VinylMix $mix, Request $request, EntityManagerInterface $entityManager): Response
    {
        $direction = $request->request->get('direction', 'up');

        if ($direction === 'up') {
            $mix->upVote(); // 修正
        } else {
            $mix->downVote(); // 修正
        }

        $entityManager->flush();
        $this->addFlash('success', 'Vote counted!');

        return $this->redirectToRoute('app_mix_show', ['id' => $mix->getId()]);
    }
```

## Rich Model vs Anemic Model

- Anemic Model:
  - "Anemic Model" とは、エンティティにビジネスロジックがない状態を指す。
  - つまり、Getter/Setter のみを持つエンティティを指す。
  - 柔軟だが、ロジックが分散しやすく、可読性が低下する。
- Rich Model:
  - 必要なロジックを Entity 内にカプセル化することで、エンティティにビジネスロジックを持たせる。
  - ロジックが集中し、可読性が向上する。
  - ただし、エンティティが肥大化する可能性がある。
