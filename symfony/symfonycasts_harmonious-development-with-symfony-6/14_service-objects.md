# 14. Service Objects

## 学習日

- 2024/10/30

## 学習リソース

- Symfonycasts: Harmonious Development with Symfony 6
- [#14 Service Objects}](https://symfonycasts.com/screencast/symfony6/services)

## Service

- 「Service」とは、依存性の管理を行うコンポーネントであり、再利用可能な機能を提供するオブジェクトのことを指す。

## `debug:autowiring`

- `debug:autowiring` コマンドを使用して、サービスの一覧を確認できる。

```bash
php bin/console debug:autowiring
```

- 特定のサービスを検索する場合は、コマンドの後ろに検索語を指定する。

```bash
php bin/console debug:autowiring log
```

- 実行しようとするタスクで、どのサービスが必要か覚えていないときに、キーワードと `debug:autowiring` で確認することができる。

## logger を使う

- `logger` サービスを使用して、ログを出力することができる。

```php
// ... lines 3 - 4
use Psr\Log\LoggerInterface;
// ... lines 6 - 10
class SongController extends AbstractController
{
    #[Route('/api/songs/{id<\d+>}', methods: ['GET'])]
    public function getSong(int $id, LoggerInterface $logger): Response
    {
// ... lines 16 - 22
        $logger->info('Returning API response for song {song}', [
            'song' => $id,
        ]);
// ... lines 24 - 25
    }
}
```
