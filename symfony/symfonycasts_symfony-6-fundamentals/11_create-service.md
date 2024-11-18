# 11. Creating a Service

## 学習日

- 2024/11/18

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#11 Creating a Service](https://symfonycasts.com/screencast/symfony6-fundamentals/create-service)

## Service の作成

- Service クラスは `src/` ディレクトリに作成する。
- 例えば、`src/Service/MixRepository.php` というファイルを作成する。

```php
<?php

namespace App\Service;

use Psr\Cache\CacheItemInterface;

class MixRepository
{
    public function findAll(): array
    {
        return $cache->get('mixes_data', function(CacheItemInterface $cacheItem) use ($httpClient) {
            $cacheItem->expiresAfter(5);
            $response = $httpClient->request('GET', 'https://raw.githubusercontent.com/SymfonyCasts/vinyl-mixes/main/mixes.json');
            return $response->toArray();
        });
    }
}
```

- サービスを作成すると、Symfony が自動で認識し、利用できるようになる。

## Service の使用

- 以下のようにして、作成したサービスをコントローラーで使用することができる。

```php
// ... lines 1 - 4
use App\Service\MixRepository; // 追加
// ... lines 6 - 12
class VinylController extends AbstractController
{
// ... lines 15 - 33
    public function browse(HttpClientInterface $httpClient, CacheInterface $cache, MixRepository $mixRepository, string $slug = null): Response // MixRepository を追加
    {
        $mixes = $mixRepository->findAll();
    {
// ... lines 36 - 43
    }
}
```
