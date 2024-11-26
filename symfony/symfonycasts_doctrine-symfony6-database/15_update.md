# 15. Updating an Entity

## 学習日

- 2024/11/26

## 学習リソース

- Symfonycasts: Doctrine, Symfony 6 & the Database
- [#15 Updating an Entity](https://symfonycasts.com/screencast/symfony-doctrine/update)

## エンティティの更新

- エンティティを更新するためには、`EntityManager` を使ってエンティティを取得し、変更を加えてから `flush()` メソッドを呼び出す。

```php
class MixController extends AbstractController
{
    public function vote(VinylMix $mix, Request $request, EntityManagerInterface $entityManager): Response
    {
        // ...
        $entityManager->flush();
        // ...
    }
}
```

- データを更新する場合は、`flush()` メソッドを使って変更をデータベースに保存する。
- `persist()` を再度呼び出す必要がない理由は以下の通り:
  - 既存のエンティティは既に Doctrine によって取得されており、管理下にあるため。
  - `flush()` は自動的に変更を検知し、`INSERT` または `UPDATE` クエリを適切に実行するため。

## 別のページへのリダイレクト

- データを更新した後、ユーザーを別のページにリダイレクトする場合は、`redirectToRoute()` メソッドを使う。

```php
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
        return $this->redirectToRoute('app_mix_show', ['id' => $mix->getId()]);
    }
```

- 第一引数にはルート名、第二引数にはルートパラメータを渡す。
