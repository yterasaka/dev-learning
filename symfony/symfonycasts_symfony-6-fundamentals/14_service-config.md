# 14. Manual Service Config in service.yaml

## 学習日

- 2024/11/19

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#14 Manual Service Config in service.yaml](https://symfonycasts.com/screencast/symfony6-fundamentals/service-config)

## services.yaml

- Auto Wiring はサービスを自動的に登録する機能。
- しかし、サービスを手動で登録することもできる。
- その場合は、`config/services.yaml` にサービスを登録する。
- 例えば、以下のようにしてサービスを登録することができる。

```yaml
// ... lines 1 - 7
services:
// ... lines 9 - 25
    App\Service\MixRepository: // サービスの名前空間
        bind:
            '$isDebug': '%kernel.debug%' // パラメータをバインド
```

- 上記のように設定したパラメータは、以下のようにして取得することができる。

```php
// ... lines 1 - 8
class MixRepository
{
    public function __construct(
// ... lines 12 - 13
        private bool $isDebug
    ) {}
    public function findAll(): array
    {
        return $this->cache->get('mixes_data', function(CacheItemInterface $cacheItem) {
            $cacheItem->expiresAfter($this->isDebug ? 5 : 60);
// ... lines 21 - 23
        });
    }
}
```
