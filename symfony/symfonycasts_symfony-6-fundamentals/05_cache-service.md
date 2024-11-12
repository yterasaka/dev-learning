# 05. The Cache Service

## 学習日

- 2024/11/12

## 学習リソース

- Symfonycasts: Symfony 6 Fundamentals: Services, Config & Environments
- [#05 The Cache Service](https://symfonycasts.com/screencast/symfony6-fundamentals/cache-service)

## CacheInterface

- HTTP リクエストの結果をキャッシュするために、`CacheInterface` サービスを使用する。
- Cache Service は以下のコマンドで見つけることができる。

```bash
php bin/console debug:autowiring cache
```

- `CacheInterface` は以下のように使用する。

```php
use Symfony\Contracts\Cache\CacheInterface;
class VinylController extends AbstractController
{
    public function browse(HttpClientInterface $httpClient, CacheInterface $cache, string $slug = null): Response
    {

        $mixes = $cache->get('mixes_data', function() use ($httpClient) {
            $response = $httpClient->request('GET', 'https://raw.githubusercontent.com/SymfonyCasts/vinyl-mixes/main/mixes.json');
            return $response->toArray();
        });

    }
}
```

## Cache Profiler を使用したデバッグ

- Cache が正しく動作しているか確認するために、`Cache Profiler` を使用する。

![alt text](<images/05_cache-service/Screenshot 2024-11-12 at 14.17.08.png>)

## Cache の保存期間の設定

- 以下のようにして Cache の保存期間を設定することができる。

```php
use Psr\Cache\CacheItemInterface;
class VinylController extends AbstractController
{
    public function browse(HttpClientInterface $httpClient, CacheInterface $cache, string $slug = null): Response
    {
        $mixes = $cache->get('mixes_data', function(CacheItemInterface $cacheItem) use ($httpClient) {
            $cacheItem->expiresAfter(5);
        });
    }
}
```

## Cache の削除

- 以下のコマンドでキャッシュのリストを表示する。

```bash
php bin/console cache:pool:list
```

- 以下のコマンドでキャッシュを削除する。
- cache.app 以外はシステムのキャッシュなので、削除しないようにする。

```bash
php bin/console cache:pool:clear cache.app
```
